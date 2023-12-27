## GENERAR HASH EN MD5 CON PYTHON
```python
import hashlib

with open("pruebas.py", "rb") as f:
    file_hash = hashlib.md5(f.read()).hexdigest()

print(file_hash)
```
![[Pasted image 20230403145639.png]]
## GENERAR HASH EN SHA1 CON PYTHON
```python
import hashlib

with open("pruebas.py", "rb") as f:
    file_hash = hashlib.sha1(f.read()).hexdigest()

print(file_hash)
```
![[Pasted image 20230403145755.png]]

