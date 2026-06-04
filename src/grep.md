# Grep

```console
grep -rnw '/path/to/somewhere/' -e 'pattern'
```

- `-r` recursive
- `-n` line number
- `-w` match the whole word.
- `-l` list the file name
- `-e` what follows is the PATTERN to search for.
- `-A <number>` print the matching line, then this many lines afterwards.

## References

[Find all files containing a specific text (string) on Linux - Stack Overflow](https://stackoverflow.com/questions/16956810/find-all-files-containing-a-specific-text-string-on-linux)
