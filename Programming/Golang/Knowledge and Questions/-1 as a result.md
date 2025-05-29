 // No complete line yet, return 0 bytes consumed and no error
if endIndex == -1 {
return "", "", "", 0, nil
}

Using `-1` as a sentinel value to indicate "not found" or "no result" is a common practice across many Go packages and standard libraries.

pkgs that use this include strings, bytes and sort.