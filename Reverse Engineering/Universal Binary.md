- A universal binary is a single executable file that encapsulates multiple Mach-O binaries, each compiled for distinct CPU architectures such as x86_64 for Intel processors and arm64 for Apple Silicon, enabling the operating system to automatically select and execute the appropriate variant at runtime based on the host hardware.

- It enables cross-architecture execution by encapsulating multiple architecture-specific executable images, known as "slices," within a single file container. This allows the same binary to run natively on different processor types without requiring separate distributions.
- Example :
```bash
$ file test
test : Mach-O universal binary with 2 architectures: [x86_64:\012- Mach-O 64-bit x86_64 executable, flags:<NOUNDEFS|DYLDLINK|TWOLEVEL|PIE>] [\012- arm64]
```
