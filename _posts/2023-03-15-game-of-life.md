---
title: Conway Game Of Life
layout: demo
date: 2023-03-15T03:05:17.910Z
preview: assets/wasm/GameOfLife/gol.html
tags:
  - gamedev
categories:
  - demo
  - pt-br
lastmod: 2023-03-17T17:13:01.965Z
---
Para o Game Of Life, nosso *espaço* será definido como
um 2d array, isso equivale a uma tela ou plano cartesiano.
O *estado* será um simples booliano, porque nesse
Autômato o estado varia apenas entre dois valores. Temos
assim o seguinte struct para armazenar nossos estados.

```go
type GameOfLife struct {
 values [][]bool
}
```

O comportamento do nosso Autômato é definido nesse método Update. Nele cada elemento do nosso array é comparado aos seus vizinhos, isso determina qual o novo estado daquela célula.

```go
func (gol *GameOfLife) Update() error {
 // Primeiro criamos uma cópia do estado original, garantindo assim que não haverá conflitos
	m0 := make([][]bool, len(gol.values))
	copy(m0, gol.values)

	for x, row := range gol.values {
		for y, v0 := range row {
			// Cell names
			// |v1|v2|v3|
			// |v4|v0|v5|
			// |v6|v7|v8|
			var alive uint

			// Aqui contamos quantos vizinhos com estado verdadeiro essa célula possui
			if v1 := gol.IsAliveOrNil(x-1, y-1); v1 != nil && *v1 {
				alive++
			}
			if v2 := gol.IsAliveOrNil(x, y-1); v2 != nil && *v2 {
				alive++
			}
			if v3 := gol.IsAliveOrNil(x+1, y-1); v3 != nil && *v3 {
				alive++
			}
			if v4 := gol.IsAliveOrNil(x-1, y); v4 != nil && *v4 {
				alive++
			}
			if v5 := gol.IsAliveOrNil(x+1, y); v5 != nil && *v5 {
				alive++
			}
			if v6 := gol.IsAliveOrNil(x-1, y+1); v6 != nil && *v6 {
				alive++
			}
			if v7 := gol.IsAliveOrNil(x, y+1); v7 != nil && *v7 {
				alive++
			}
			if v8 := gol.IsAliveOrNil(x+1, y+1); v8 != nil && *v8 {
				alive++
			}

			// Essa é aplicação da regra em si. A definição dela é a clássico proposta por Conway.
			// Uma célula viva morre quando possui menos de 2 vizinhos vivos
			// Uma célula viva morre quando possui mais de 3 vizinhos vivos
			// Uma célula morta volta a vida quando possui exatamente 3 vizinhos vivos
			// Em todos os outros casos ela mantém seu estado original
			if v0 && (alive < 2 || alive > 3) { 
				m0[x][y] = false
			}
			if !v0 && (alive == 3) {
				m0[x][y] = true
			}

		}
	}
	gol.values = m0
	return nil
}
```

Para determinar se a célula existe e qual o seu estado, existe esse método. Perceba que ele retorna um ponteiro, caso a célula não exista é retornado um ponteiro vazio(nil), do contrário é retornado o estado daquela célula.

```go
func (gol *GameOfLife) IsAliveOrNil(x, y int) *bool {
	xOut := x < 0 || x > len(gol.values)-1
	yOut := y < 0 || y > len(gol.values)-1

	if xOut || yOut {
		return nil
	}
	return &gol.values[x][y]
}
```
