INTO static demo - transfer parts

These four binary files are consecutive parts of one verified static-site ZIP.
They are not separate ZIP archives. Download ALL four parts into one directory.
No backend, package.json, server.js, API key, or build step is needed to host the
reassembled static site. Its resource paths support /s/{appid}/ hosting.

Reassemble with Python 3, run from the directory containing all four parts:

from pathlib import Path
import hashlib
parts = [Path(f"into-static-redcowork.zip.part{i:02d}") for i in range(1, 5)]
out = Path("into-static-redcowork.zip")
assert not out.exists(), "Output already exists; choose an empty directory."
with out.open("wb") as target:
    for part in parts:
        target.write(part.read_bytes())
assert out.stat().st_size == 70930062
assert hashlib.sha256(out.read_bytes()).hexdigest() == "c67996b8509228ef2827a1d2f59502d9768da105526b5c454e9529874b9c4499"
print("Verified ZIP:", out)

Then extract the ZIP into the deployment's empty static-site root. Preserve all
relative filenames and directories. The ZIP contains prebuilt index.html, JS,
CSS and local media, not the original React/TSX source project.

CHECKSUMS.txt records each part's SHA-256 and the reassembled ZIP's SHA-256.
The first three parts are 19 MiB each; the fourth is the remaining bytes.
Do not upload the .part files as the deployed website itself.
