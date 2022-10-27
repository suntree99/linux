## Package Regexp
```go
import "regexp"
import "fmt"

func main() {

	var regex *regexp.Regexp = regexp.MustCompile("e([a-z])a")
	fmt.Println(regex.MatchString("eka")) // true
	fmt.Println(regex.MatchString("eta")) // true
	fmt.Println(regex.MatchString("eDa")) // false

	search := regex.FindAllString("eka eko eda eta eya eki", -1)
	fmt.Println(search) // [eka eda eta eya]
}
```

##
##

# Heading 1 / Judul Utama (gunakan #)

## Heading 2 / Sub Judul (gunakan ##)

Text biasa (ditulis biasa tanpa format apapun)

[Hyperlink](https://www.google.com) (nama hyperlink dibungkus kurung siku, urlnya dibungkus tanda kurung biasa)

```bash
git add .
git commit -m "baris code menggunakan backtick 3x di awal(sertakan bahasanya) dan akhir code"
git push
```

Untuk `menyoroti` bungkus text dengan backtick 1x

## Package 
```go
Isi
```

Update README.md
