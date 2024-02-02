# run a subprocess and capture its CONOUT$ console output

## Tested against Windows / Python 3.11 / Anaconda

## pip install subprocconout

```
subprocess_conout(*args, nrows=9999, encode=True, **kwargs):
    Function to run a subprocess and capture its CONOUT$ console output.

    Args:
        *args: Variable length argument list.
        nrows: Number of rows to capture from the console output (default is 9999).
        encode: Boolean indicating whether to encode the console output (default is True).
        **kwargs: Variable length keyword argument list.

    Returns:
        The captured console output.
		
from subprocconout import subprocess_conout
import textwrap
# based on https://stackoverflow.com/a/38749458/15096247
text = textwrap.dedent(
    """\
    Lorem ipsum dolor sit äçamet, consectetur adipiscing elit, sed do
    eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
    enim ad minim veniam, quis nostrud exercitation ullamco laboris
    nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
    in reprehenderit in voluptate velit esse cillum dolore eu
    fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est
    laborum."""
)

cmd = (
    'python -c "'
    "print('piped output');"
    "conout = open(r'CONOUT$', 'w');"
    "conout.write('''%s''')\"" % text
)

result = subprocess_conout(cmd, nrows=9999, encode=True)
print([[q for q in line.split(b"\x00") if q] for line in result if line])

# [
#     [
#         b"Lorem ipsum dolor sit \x84\x87amet, consectetur adipiscing elit, sed do",
#         b"eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut",
#     ],
#     [
#         b"enim ad minim veniam, quis nostrud exercitation ullamco laboris",
#         b"nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor",
#     ],
#     [
#         b"in reprehenderit in voluptate velit esse cillum dolore eu",
#         b"fugiat nulla pariatur. Excepteur sint occaecat cupidatat non",
#     ],
#     [
#         b"proident, sunt in culpa qui officia deserunt mollit anim id est",
#         b"laborum.",
#     ],
# ]
		
```
