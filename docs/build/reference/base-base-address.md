---
description: "Learn more about: /BASE (Base address)"
title: "/BASE (Base address)"
ms.date: 03/27/2025
f1_keywords: ["/base", "VC.Project.VCLinkerTool.BaseAddress"]
helpviewer_keywords: ["base addresses [C++]", "programs [C++], preventing relocation", "semicolon [C++], specifier", "-BASE linker option", "key address size", "environment variables [C++], LIB", "programs [C++], base address", "LIB environment variable", "BASE linker option", "DLLs [C++], linking", "/BASE linker option", "@ symbol for base address", "executable files [C++], base address", "at sign symbol for base address"]
---
# `/BASE` (Base address)

Specifies the base address for a program.

## Syntax

> **`/BASE:`**{*`address`*[**`,`***`size`*] | **`@`***`filename`***`,`***`key`*}

## Remarks

> [!NOTE]
> For security reasons, Microsoft recommends you use the [`/DYNAMICBASE`](dynamicbase-use-address-space-layout-randomization.md) option instead of specifying base addresses for your executables. **`/DYNAMICBASE`** generates an executable image that can be randomly rebased at load time by using the address space layout randomization (ASLR) feature of Windows. The **`/DYNAMICBASE`** option is on by default.

The **`/BASE`** linker option sets a base address for the program. It overrides the default location for an EXE or DLL file. The default base address for an EXE file is 0x400000 for 32-bit images or 0x140000000 for 64-bit images. For a DLL, the default base address is 0x10000000 for 32-bit images or 0x180000000 for 64-bit images. On operating systems that don't support address space layout randomization (ASLR), or when the **`/DYNAMICBASE:NO`** option was set, the operating system first attempts to load a program at its specified or default base address. If insufficient space is available there, the system relocates the program. To prevent relocation, use the [`/FIXED`](fixed-fixed-base-address.md) option.

The linker issues an error if *`address`* isn't a multiple of 64K. You can optionally specify the size of the program. The linker issues a warning if the program can't fit in the size you specified.

On the command line, another way to specify the base address is by using a *base address response file*. A base address response file is a text file that contains the base addresses and optional sizes of all the DLLs your program uses, and a unique text key for each base address. To specify a base address by using a response file, use an at sign (**`@`**) followed by the name of the response file, *`filename`*, followed by a comma, then the *`key`* value for the base address to use in the file. The linker looks for *`filename`* in either the specified path, or if no path is specified, in the directories specified in the `LIB` environment variable. The fully qualified *`filename`* must not exceed `MAX_PATH` (260 characters). Each line in *`filename`* represents one DLL and has the following syntax:

> *`key`* *`address`* \[*`size`*] **`;`** *`comment`*

The *`key`* is a string of alphanumeric characters and isn't case sensitive. It's usually the name of a DLL, but that's not required. The *`key`* is followed by a base *`address`* in C-language, hexadecimal, or decimal notation and an optional maximum *`size`*. All three arguments are separated by spaces or tabs. The linker issues a warning if the specified *`size`* is less than the virtual address space required by the program. A *`comment`* is specified by a semicolon (**`;`**) and can be on the same or a separate line. The linker ignores all text from the semicolon to the end of the line. This example shows part of such a file:

```txt
main   0x00010000    0x08000000    ; for PROJECT.exe
one    0x28000000    0x00100000    ; for DLLONE.DLL
two    0x28100000    0x00300000    ; for DLLTWO.DLL
```

If the file that contains these lines is called `DLLS.txt`, the following example command applies this information:

```cmd
link dlltwo.obj /dll /base:@dlls.txt,two
```

Another way to set the base address is by using the *`BASE`* argument in a [`NAME`](name-c-cpp.md) or [`LIBRARY`](library.md) statement. The **`/BASE`** and [`/DLL`](dll-build-a-dll.md) options together are equivalent to the **`LIBRARY`** statement.

### To set this linker option in the Visual Studio development environment

1. Open the project's **Property Pages** dialog box. For details, see [Set C++ compiler and build properties in Visual Studio](../working-with-project-properties.md).
1. Select the **Configuration Properties** > **Linker** > **Advanced** property page.
1. Modify the **Base Address** property.

### To set this linker option programmatically

- See <xref:Microsoft.VisualStudio.VCProjectEngine.VCLinkerTool.BaseAddress%2A>.

## See also

[MSVC linker reference](linking.md)\
[MSVC linker options](linker-options.md)
