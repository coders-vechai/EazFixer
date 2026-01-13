# EazFixer
A deobfuscation tool for Eazfuscator.

> [!WARNING]
> EazFixer works by invoking code in the binary it operates on. Either due to fuzzy heuristics or intentional effort on the obfuscator's part, EazFixer may execute code in the binary that is malicious in nature. Never use EazFixer on untrusted binaries unless you are in an insulated environment. Assume that running EazFixer is equivalent to running the binary.

## Description
EazFixer is a deobfuscation tool for [Eazfuscator](https://www.gapotchenko.com/eazfuscator.net), a commercial .NET obfuscator. For a list of features, see the list below.

### Implemented fixes:
* String encryption
* Resource encryption
* Assembly embedding

### Not implemented, may be added in the future:
* Entrypoint obfuscation
* Data virtualization

### Out of scope:
* Code virtualization (consider using [EazyDevirt](https://github.com/puff/EazyDevirt))
* Symbol renaming (symbol names are either unrecoverable or encrypted. For symbol decryption in case of a known key, see [EazDecode](https://github.com/HoLLy-HaCKeR/EazDecode))
* Automatic code optimization (not an anti-feature!)
* Code control flow obfuscation (I didn't have any problems with my samples in dnSpy)
* Assemblies merging (doesn't seem probable, especially with symbol renaming)
* Control flow obfuscation (use de4dot)

## Usage
Call from the command line or drag and drop the file on and let it run or use the command line flag `--file`.

If your assembly is protected with control-flow obfuscation, run it through [de4dot](https://github.com/0xd4d/de4dot) with the
`--only-cflow-deob` flag first. This could break the AssemblyResolver processor's method detection, so you may need to manually find the AssemblyResolver tokens and use the `--asmres-decrypt-tok` and `--asmres-decompress-tok` flags.

* --file path
    * input file
* --keep-types
    * similar to the de4dot flag, keeps obfuscator types and assemblies
* --virt-fix
    * keeps certain parts obfuscated to stay working with [virtualized](https://help.gapotchenko.com/eazfuscator.net/30/virtualization) assemblies.
* --preserve-all
    * preserves all metadata

The following flags are for manually specifying metadata tokens in the case EazFixer fails to find them. They accept the token in hex format. e.g. `--str-decrypt-tok 0x060002B7`
* --str-decrypt-tok
    * string decryption method
* --res-resolver-tok
    * resource resolver type
* --res-init-tok
    * resource initialization method
* --asmres-type-tok
    * assembly resolver type
* --asmres-movenext-tok
    * assembly resolver's MoveNext implementation method token
* --asmres-decrypt-tok
    * assembly resolver's decryption method
* --asmres-decompress-tok
    * assembly resolver's decompression method

example: `EazFixer.exe --file test.exe --keep-types`

## Building
Clone the repository and use the latest version of Visual Studio (2019, at the time of writing).

## Support
EazFixer is (and will always be) targeted at the latest version of Eazfuscator. If your version is not supported, try a more universal
deobfuscator like [de4dot](https://github.com/0xd4d/de4dot). If your version is newer than what this tool supports, create an issue only
**after** verifying with the latest version of Eazfuscator.

Also, I will not help you use this program. Consider it for advanced users only. If you do run into a problem and are sure it is a bug,
feel free to submit an issue but I cannot guarantee I will fix it.

## Related projects
- [EazDecode](https://github.com/HoLLy-HaCKeR/EazDecode), for decrypting encrypted symbol names in case of a known encryption key.
- [eazdevirt](https://github.com/saneki/eazdevirt), a tool for devirtualizing older version of EazFuscator.
- [eazdevirt fork](https://github.com/HoLLy-HaCKeR/eazdevirt), my abandoned fork of eazdevirt, may work slightly better on newer samples.

## Credits
This tool uses the following (open source) software:
* [dnlib](https://github.com/0xd4d/dnlib) by [0xd4d](https://github.com/0xd4d), licensed under the MIT license, for reading/writing assemblies.
* [Harmony](https://github.com/pardeike/Harmony) by [Andreas Pardeike](https://github.com/pardeike), licensed under the MIT license, for patching the stacktrace which allows for reflection invocation to be used.
