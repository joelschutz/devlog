---
title: Diffusion Automata v1
layout: demo
date: 2023-03-24T01:04:38.048Z
preview: assets/wasm/Automata/automata.html
wasm: assets/wasm/Automata/automata.v1.wasm
tags:
  - gamedev
  - go-terrain
categories:
  - demo
lastmod: 2023-03-24T01:12:52.141Z
---
A ideia desse Automata é simular a dispersão de um
liquido em um meio solido permeável, como solo, areia, etc.
Ele foi criado como protótipo de um sistema de dispersão de
água subterrânea para meu jogo.

Como vai perceber, não existe muita complexidade nesse código,
essa é a magica desse tipo de algoritmo, com regras simples é
possível construir comportamentos complexos. Aqui, simulamos
a interação entre líquidos e sólidos no tempo e espaço usando apenas
aritmética básica.

Nosso espaço será representado como um 2d array de vetores, mas
no nosso caso eles servem apenas como uma forma conveniente de
agrupar nossos dois valores relevantes:

- Umidade
  - A quantidade de liquido contido naquela célula
- Impermeabilidade
  - A resistência que o material daquela célula apresenta ao movimento de liquido

Os campos `Rocks` e `Rain` servem para demonstrar, respectivamente,
células totalmente impermeáveis e células que sao fontes de umidade.

```go
type HumidityBoard struct {
	values      [][]mgl32.Vec2 // [humidity, impermeability]
	Rocks, Rain [][]bool
	hvrX, hvrY  int
}
```

A lógica para determinar a umidade de uma célula é bastante simples,
entendendo que ao longo do tempo o liquido tende a se espalhar uniformemente
pelo espaço, assumimos que o nosso valor de umidade da célula é a média aritmética
entre o seu valor atual e a das suas vizinhas. Por simplicidade assumimos apenas
4 vizinhos conforme esse diagrama onde `v0` é a célula atual:

|   |v3|
|v1|v0|v2|
|   |v4|

Para considerar a permeabilidade, utilizamos uma média ponderada onde
o peso de cada termo é definido pelo valor de impermeabilidade daquela célula.
O detalhe é que consideramos o reciproco(1/valor) como peso das células vizinhas,
dessa forma conseguimos o efeito de que uma célula altamente impermeável tende
a não perder umidade ao mesmo tempo que resiste à absorção de mais liquido. Além
disso, verificamos se a célula excede o nosso limite de 1024 e ajustamos então o valor para esse limite.

```go
func (ba *HumidityBoard) Update() error {
	// Armazenamos o estado inicial do espaço para servir de referencia
	m0 := make([][]mgl32.Vec2, len(ba.values))
	copy(m0, ba.values)

	for x, row := range ba.values {
		for y, v0 := range row {
			// Pulamos o calculo de fontes de umidade e células com alto impermeabilidade
			if ba.Rain[x][y] || v0[1] >= (math.MaxFloat32/5)*4 {
				continue
			}

			// Assumimos que as bordas são células secas e impermeáveis
			v1 := mgl32.Vec2{0, math.MaxFloat32}
			v2 := mgl32.Vec2{0, math.MaxFloat32}
			v3 := mgl32.Vec2{0, math.MaxFloat32}
			v4 := mgl32.Vec2{0, math.MaxFloat32}

			// Verificamos se o vizinho existe e aplicamos os valores corretos
			if x > 0 {
				v1 = m0[x-1][y]
			}
			if x < len(m0)-1 {
				v2 = m0[x+1][y]
			}
			if y > 0 {
				v3 = m0[x][y-1]
			}
			if y < len(m0)-1 {
				v4 = m0[x][y+1]
			}

			// Calculamos a média aritmética ponderada
			r := ((v0[0] * (v0[1])) + (v1[0] / v1[1]) + (v2[0] / v2[1]) + (v3[0] / v3[1]) + (v4[0] / v4[1])) / (v0[1] + (1 / v1[1]) + (1 / v2[1]) + (1 / v3[1]) + (1 / v4[1]))

			// Limitamos os valores a um máximo de 1024
			if r > 1024 {
				r = 1024
			}

			// Atualizamos espaço com novo valor de umidade
			ba.values[x][y][0] = r
		}
	}
	return nil
}
```

O resultado no fim não é perfeito, há parâmetros que não são levados em consideração como
velocidade e densidade do liquido, mas para o nosso caso já é suficiente. Outra limitação
é quanto a conservação de massa do sistema. Aos poucos o volume total de umidade cai e isso
causa o efeito de umidade desaparecendo espontaneamente, que é fisicamente impossível.

Como dito, esse é um protótipo e limitações como essa não são necessariamente problemas
para aplicação em jogos. Vale lembrar que esse algoritmo foi escrito de forma síncrona,
mas é totalmente possível adapta-lo para operar de forma paralelizada.
