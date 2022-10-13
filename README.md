# rangeset-exec

The rangeset-exec tool can generate CLI command strings and also execute such commands with [clustershell](https://clustershell.readthedocs.io/en/latest/) [RangeSet](https://clustershell.readthedocs.io/en/latest/api/RangeSet.html) support for decimal and hexadecimal numbers.  
Range sets with hexadecimal numbers must be specified with preleading `0x` or `0X`.

To specify the range set to be used a placeholder must be specified in the command string with `{0}`.  
Currently only one placeholder is supported.

## Options

```
usage: rangeset-exec.py [-h] -c CMD -r RANGESET [-f FORMAT_HEX] [-p] [-x] [-v]

Execution of CLI commands with clustershell RangeSet support for decimal and hexadecimal numbers

options:
  -h, --help            show this help message and exit
  -c CMD, --command CMD
                        Set command to be executed with positional rangeset placeholder: '{0}'
  -r RANGESET, --rangeset RANGESET
                        Specify decimal or hexadecimal range set e.g. '0-10,27-45,60-100/2' or '0x00ab,0x00cd-0x00ff'
  -f FORMAT_HEX, --format-hex FORMAT_HEX
                        Specify Python string format to generate hex numbers (default: {:04x})
  -p, --print-mode      Enable print mode to show commands generated
  -x, --use-hex         Enable use of hex numbers for range set
  -v, --version         show program's version number and exit
```

## Print Examples

### Numbers Only

**Decimal**

```bash
$ ./rangeset-exec.py -r '0-2,20-22,60-64/2' -c "{0}" -P
0
...
62
64
```

**Hexadecimal**

```bash
$ ./rangeset-exec.py -r '0-2,20-22,60-64/2' -c "{0}" -f "0x{:08x}" -X -P 
0x00000000
...
0x0000003e
0x00000040
```

### With LCTL Command

```bash
$ ./rangeset-exec.py -r "0x00cd-0x00ff" -c "lctl get_param osc.hebe-OST{0}-osc-MDT0000.active" -X
osc.hebe-OST00cd-osc-MDT0000.active=1
...
osc.hebe-OST00fe-osc-MDT0000.active=1
osc.hebe-OST00ff-osc-MDT0000.active=1
```
