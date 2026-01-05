# ft_printf - 42 School Printf Implementation

> C standart kütüphanesindeki printf fonksiyonunun variadic functions kullanılarak yeniden yazılması.

## 📋 İçindekiler

- [Proje Hakkında](#-proje-hakkında)
- [Desteklenen Format Specifiers](#-desteklenen-format-specifiers)
- [Kurulum](#-kurulum)
- [Kullanım](#-kullanım)
- [Nasıl Çalışır](#-nasıl-çalışır)
- [Test](#-test)
- [Kaynaklar](#-kaynaklar)

---

## 🎯 Proje Hakkında

**ft_printf**, 42 School müfredatının temel projelerinden biridir. Bu projede, C dilinin en çok kullanılan fonksiyonlarından biri olan `printf`'in kendi versiyonumuzu yazıyoruz.

Proje, **variadic functions** (`va_start`, `va_arg`, `va_copy`, `va_end`) kullanımını öğretirken aynı zamanda format string parsing ve tip dönüşümleri konularında derinlemesine bilgi kazandırır.

### Öğrenilen Kavramlar

- Variadic functions ve `<stdarg.h>` kütüphanesi
- Format string parsing
- Tamsayıdan string'e dönüşüm (farklı tabanlarda)
- Pointer adresi yazdırma
- Karakter ve string işlemleri
- Statik kütüphane oluşturma

---

## 📚 Desteklenen Format Specifiers

### Mandatory Part

| Specifier | Açıklama | Örnek |
|-----------|----------|-------|
| `%c` | Tek karakter yazdırır | `ft_printf("%c", 'A')` → `A` |
| `%s` | String yazdırır | `ft_printf("%s", "Hello")` → `Hello` |
| `%p` | Pointer adresi yazdırır (hex) | `ft_printf("%p", ptr)` → `0x7fff5fbff8c8` |
| `%d` | Decimal (10 tabanında) tamsayı | `ft_printf("%d", 42)` → `42` |
| `%i` | Integer (10 tabanında) tamsayı | `ft_printf("%i", -42)` → `-42` |
| `%u` | Unsigned decimal tamsayı | `ft_printf("%u", 42)` → `42` |
| `%x` | Hexadecimal (küçük harf) | `ft_printf("%x", 255)` → `ff` |
| `%X` | Hexadecimal (büyük harf) | `ft_printf("%X", 255)` → `FF` |
| `%%` | Literal yüzde işareti | `ft_printf("%%")` → `%` |

```

---

## ⚙️ Kurulum

### Gereksinimler

- GCC derleyici
- Make
- Unix tabanlı işletim sistemi (Linux/macOS)

### Derleme

```bash
# Repository'yi klonla
git clone https://github.com/KULLANICI_ADI/ft_printf.git

# Dizine gir
cd ft_printf

# Kütüphaneyi derle
make

# Bonus ile derle
make bonus

# Temizlik
make clean    # .o dosyalarını sil
make fclean   # .o ve .a dosyalarını sil
make re       # Yeniden derle
```

---

## 🚀 Kullanım

### Projenize Dahil Etme

```c
#include "ft_printf.h"

int main(void)
{
    int count;
    char *str = "World";
    int num = 42;
    
    // Basit kullanım
    ft_printf("Hello, %s!\n", str);
    
    // Birden fazla specifier
    ft_printf("String: %s, Number: %d, Hex: %x\n", str, num, num);
    
    // Pointer adresi
    ft_printf("Address of num: %p\n", &num);
    
    // Return değeri (yazılan karakter sayısı)
    count = ft_printf("Test message\n");
    ft_printf("Printed %d characters\n", count);
    
    return (0);
}
```

### Derleme

```bash
gcc -Wall -Wextra -Werror main.c -L. -lftprintf -o program
```

### Çıktı

```
Hello, World!
String: World, Number: 42, Hex: 2a
Address of num: 0x7ffd5e8e3f4c
Test message
Printed 13 characters
```

---

## 🔍 Nasıl Çalışır

### Genel Akış

```
┌─────────────────┐
│   ft_printf     │
│  "Hello %s!"    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Format String  │
│    Parsing      │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────┐
│ Text  │ │ % + ? │
│"Hello"│ │  %s   │
└───┬───┘ └───┬───┘
    │         │
    ▼         ▼
┌───────┐ ┌────────────┐
│ write │ │ va_arg +   │
│       │ │ handler    │
└───┬───┘ └─────┬──────┘
    │           │
    └─────┬─────┘
          ▼
    ┌───────────┐
    │  Output   │
    │"Hello World!"│
    └───────────┘
```

### Variadic Functions Kullanımı

```c
#include <stdarg.h>

int	ft_printf(const char *str, ...)
{
	va_list	arg;
	int		i;
	int		check;

	va_start(arg, str);
	i = 0;
	check = 0;
	while (str[i])
	{
		if (str[i] != '%')
		{
			ft_putchar(str[i]);
			check++;
		}
		if (str[i] == '%')
		{
			i++;
			while (str[i] == ' ' && str[i])
				i++;
			check += ft_check(str[i], arg);
		}
		i++;
	}
	va_end(arg);
	return (check);
}

// Örnek handler
int handle_format(char specifier, va_list args)
{
    if (specifier == 'd' || specifier == 'i')
        return (ft_print_int(va_arg(args, int)));
    if (specifier == 's')
        return (ft_print_str(va_arg(args, char *)));
    // ... diğer specifierlar
}
```

---

## 🧪 Test

### Test Araçları

| Test | Link |
|------|------|
| printfTester | [github.com/Tripouille/printfTester](https://github.com/Tripouille/printfTester) |
| ft_printf_tester | [github.com/paulo-music/ft_printf_tester](https://github.com/paulo-music/ft_printf_tester) |
| printf-tester | [github.com/chronorose/printf-tester](https://github.com/chronorose/printf-tester) |

### Manuel Test

```c
#include <stdio.h>
#include "ft_printf.h"

int main(void)
{
    int ret1, ret2;
    
    // Karşılaştırmalı test
    printf("--- Original printf ---\n");
    ret1 = printf("Char: %c, String: %s, Int: %d\n", 'A', "test", 42);
    
    printf("--- ft_printf ---\n");
    ret2 = ft_printf("Char: %c, String: %s, Int: %d\n", 'A', "test", 42);
    
    printf("Return values: printf=%d, ft_printf=%d\n", ret1, ret2);
    
    // Edge cases
    ft_printf("NULL string: %s\n", NULL);
    ft_printf("Negative: %d\n", -2147483648);
    ft_printf("Unsigned max: %u\n", 4294967295);
    ft_printf("Hex: %x, HEX: %X\n", 255, 255);
    ft_printf("Pointer NULL: %p\n", NULL);
    
    return (0);
}
```

### Test Çalıştırma

```bash
# printfTester
git clone https://github.com/Tripouille/printfTester.git
cd printfTester
make m    # Mandatory tests
make b    # Bonus tests
make a    # All tests
```

---

## ⚠️ Edge Cases

| Durum | Beklenen Davranış |
|-------|-------------------|
| `%s` ile `NULL` | `(null)` yazdırılır |
| `%p` ile `NULL` | `(nil)` veya `0x0` (sisteme bağlı) |
| `%d` ile `INT_MIN` | `-2147483648` |
| `%u` ile `UINT_MAX` | `4294967295` |
| `%%` | Tek `%` karakteri |
| Geçersiz specifier | Undefined behavior (veya karakteri yazdır) |

---

## 📖 Kaynaklar

- [printf(3) - Linux Manual](https://man7.org/linux/man-pages/man3/printf.3.html)
- [Variadic Functions in C](https://en.cppreference.com/w/c/variadic)
- [stdarg.h Reference](https://pubs.opengroup.org/onlinepubs/009695399/basedefs/stdarg.h.html)
- [Format Specifiers in C](https://www.cplusplus.com/reference/cstdio/printf/)

---

---

## 👤 İletişim

**42 Intra:** [malbayra]

**GitHub:** [@HalilAlb](https://github.com/HalilAlb)

---

<div align="center">

⭐ Bu proje faydalı olduysa yıldız vermeyi unutmayın!

Made with ❤️ at 42 School

</div>
