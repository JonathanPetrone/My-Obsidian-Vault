```
package main

import (
	"image"
	"image/color"
	"image/gif"
	"io"
	"log"
	"math"
	"math/rand"
	"net/http"
)

var palette = []color.Color{color.White, color.Black}

const (
	whiteIndex = 0
	blackIndex = 1
)

func main() {
	
	handler := func(w http.ResponseWriter, r *http.Request) {
		lissajous(w)
	}
	http.HandleFunc("/", handler)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
	
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

1.12 Modify the lissajous server to read parameter values from the URL.
*Not done yet*

![[Pasted image 20231125190749.png]]
från server3 när man skickar med parametrar dyker form["hej"] = ["1"] upp.