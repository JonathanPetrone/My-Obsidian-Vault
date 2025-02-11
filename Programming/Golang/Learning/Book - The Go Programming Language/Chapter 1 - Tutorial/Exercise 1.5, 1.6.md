#### 1.5 - Change color on black
```
package main

import (
"image"
"image/color"
"image/gif"
"io"
"math"
"math/rand"
"os"
)

var palette = []color.Color{color.White, color.RGBA{0x2e, 0xcc, 0x71, 0x1}}

const (
	whiteIndex = 0
	shamrockIndex = 1
)

func main() {
	lissajous(os.Stdout)
}

func lissajous(out io.Writer) {

	const (
		cycles = 5
		res = 0.001
		size = 100
		nframes = 64
		delay = 8
	)

	freq := rand.Float64() * 3.0
	anim := gif.GIF{LoopCount: nframes}
	phase := 0.0

	for i := 0; i < nframes; i++ {
		rect := image.Rect(0, 0, 2*size+1, 2*size+1)
		img := image.NewPaletted(rect, palette)

		for t := 0.0; t < cycles*2*math.Pi; t += res {
			x := math.Sin(t)
			y := math.Sin(t*freq + phase)

			img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5), blackIndex)
		}

	phase += 0.1
	anim.Delay = append(anim.Delay, delay)
	anim.Image = append(anim.Image, img)
	}
	gif.EncodeAll(out, &anim) //NOTE: ignoring encoding errors
}
```

#### 1.6 Add more colors and try them out
```
package main

import (
"image"
"image/color"
"image/gif"
"io"
"math"
"math/rand"
"os"
)

var palette = []color.Color{color.White, color.RGBA{0x2e, 0xcc, 0x71, 0x1}}

const (
whiteIndex = 0
shamrockIndex = 1
laser_CanaryIndex = 2
)

func main() {
	lissajous(os.Stdout)
}

func lissajous(out io.Writer) {

	const (
		cycles = 5
		res = 0.001
		size = 100
		nframes = 64
		delay = 8
	)

	freq := rand.Float64() * 3.0
	anim := gif.GIF{LoopCount: nframes}
	phase := 0.0

	for i := 0; i < nframes; i++ {
		rect := image.Rect(0, 0, 2*size+1, 2*size+1)
		img := image.NewPaletted(rect, palette)

		for t := 0.0; t < cycles*2*math.Pi; t += res {
			x := math.Sin(t)
			y := math.Sin(t*freq + phase)

			img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5), laser_CanaryIndex)
		}

	phase += 0.1
	anim.Delay = append(anim.Delay, delay)
	anim.Image = append(anim.Image, img)
	}
	gif.EncodeAll(out, &anim) //NOTE: ignoring encoding errors
}
```

print new lissajous by using:

go build
./exercise_1.5 >out.gif