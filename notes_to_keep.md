```bash
TIME_STAMP=date +"%d%m%Y_%H%M"

tar -xzvf [file_$TIME_STAMP.tar.gz] [file_to_compress]


tar -xzvf [file_$(date +"%d%m%Y_%H%M").tar.gz] [file_to_compress]
```
