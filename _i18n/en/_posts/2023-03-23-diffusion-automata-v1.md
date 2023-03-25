---
title: Diffusion Automata v1
layout: demo
date: 2023-03-24T01:04:38.048Z
preview: assets/wasm/Automata/automata.html
wasm: assets/wasm/Automata/automata.v1.wasm
tags:
  - gamedev
  - go-terrain
  - new
categories:
  - demo
lastmod: 2023-03-24T01:33:55.077Z
---
The goal for this Automata is to simulate the diffusion of
a liquid on a solid medium, like soil, sand, etc. It was created
as a prototype of a underground water diffusion for my game.

As will notice, there isn't much complexity in this code, that's the magic
of this kind of algorithm, we can use simple rules to create complex
behavior. Here, we simulate the interaction between liquids ans solids
over time and space using basic arithmetic

Our board will be represented by a 2d array of vectors, this is just
for convenience since we can easily group the two relevant values:

- Humidity
  - The amount of liquid within the cell
- Impermeability
  - The resistance of the cell to the movement of liquid trough it

The fields `Rocks` and `Rain` just represent cells that are totally impermeable
or sources of humidity, respectively.

```go
type HumidityBoard struct {
	values      [][]mgl32.Vec2 // [humidity, impermeability]
	Rocks, Rain [][]bool
	hvrX, hvrY  int
}
```

The logic to determine the humidity of a cell is simples, since the liquid
converges to a uniform distribution over time for all the space, we assume
that the new value of humidity of a cell is just the mean between it and
their neighbors values. For simplicity, we only count 4 neighbors as the
diagram below, where `v0` is the current cell:

|   |v3|
|v1|v0|v2|
|   |v4|

To include permeability properties, we use a weighted mean where each
factor is defined by that cell impermeability. The catch is that we use
the inverse(1/value) for neighboring cells, that way we achieve the effect
of a highly impermeable cell resisting either losing or gaining humidity.
Then we clamp to new value to a max of 1024.

```go
func (ba *HumidityBoard) Update() error {
	// Store the initial state as reference
	m0 := make([][]mgl32.Vec2, len(ba.values))
	copy(m0, ba.values)

	for x, row := range ba.values {
		for y, v0 := range row {
			// Skip the calculations for humidity sources and impermeable cell
			if ba.Rain[x][y] || v0[1] >= (math.MaxFloat32/5)*4 {
				continue
			}

			// Assume that borders are dry and impermeable
			v1 := mgl32.Vec2{0, math.MaxFloat32}
			v2 := mgl32.Vec2{0, math.MaxFloat32}
			v3 := mgl32.Vec2{0, math.MaxFloat32}
			v4 := mgl32.Vec2{0, math.MaxFloat32}

			// Verify if neighbor exists and use their value
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

			// Calculate the weighted mean
			r := ((v0[0] * (v0[1])) + (v1[0] / v1[1]) + (v2[0] / v2[1]) + (v3[0] / v3[1]) + (v4[0] / v4[1])) / (v0[1] + (1 / v1[1]) + (1 / v2[1]) + (1 / v3[1]) + (1 / v4[1]))

			// Clamp values to 1024
			if r > 1024 {
				r = 1024
			}

			// Updates board with new humidity value
			ba.values[x][y][0] = r
		}
	}
	return nil
}
```

The final result is not perfect, there are parameters that are not accounted like velocity
and density of the liquid, but it's sufficient for our porpoises. Another limitation of this
method is lack of conservation of mass. As the simulation evolves the humidity spontaneously
drops, which is physically impossible.

As stated previous, this is a prototype and limitation as this are not necessarily concerns
for games development. Keep in mind that this algorithm is synchronous, but it's totally
possible to paralelize it.

---

Hope you liked this material and that it was helpful, I'm still working on this website and demos to help people that are interested in programming things beyond the web development niche. If you have any question or suggestion, please contact me through [GitHub](https://github.com/joelschutz) or [e-mail](mailto:joelsschutz@yahoo.com.br).
![thank you!](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNDU3M2ZhZmUxZDQ4NGM3NGY1YjJlMzFkZmNkYTA2NmFhZGExNGFiNCZjdD1n/htebeL9yH0ZI9K47Jo/giphy.gif)
