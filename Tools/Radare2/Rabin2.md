rabin2 is radare2’s binary inspection tool (it's like readelf + nm + objdump summaries in one).
How to use :
```bash
rabin2 -I binary
# General info: arch, bits, PIE, NX hints, stripped, interpreter

rabin2 -Ij binary
# Same as -I but JSON (scriptable)

rabin2 -e binary
# Entry point + basic execution metadata

rabin2 -l binary
# Linked libraries + interpreter (ld-linux)

rabin2 -S binary
# Sections & segments (permissions, RELRO clues)

rabin2 -Sj binary
# Sections in JSON

rabin2 -s binary
# Symbols (functions/objects if not stripped)

rabin2 -i binary
# Imports (strcmp, write, read, malloc, etc.)

rabin2 -ij binary
# Imports in JSON

rabin2 -E binary
# Exports (rarely useful in CTFs, but quick check)

rabin2 -z binary
# Strings (printable only)

rabin2 -zz binary
# ALL strings with addresses (most useful for flags)

rabin2 -zzj binary
# Strings in JSON

rabin2 -r binary
# Relocations (GOT/PLT targets, patching help)

rabin2 -F binary
# Function list (heuristic, even when stripped)

rabin2 -C binary
# Classes / RTTI (C++ / ObjC targets)

rabin2 -H binary
# File hashes (fingerprinting, diff sanity)

rabin2 -p binary
# Program headers (NX, GNU_STACK, RELRO evidence)

rabin2 -O binary
# Extract objects/sections when possible

rabin2 -x binary
# Extract embedded files/resources (mostly PE)

```

