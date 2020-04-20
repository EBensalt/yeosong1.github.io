

# UTP-8

유니코드(국제 표준 문자표)를 사용하는 가변길이 인코딩방식

| decimal| binary | hexa | format | example |
|:---|:---|:---|:---|:---|
| 63	 |	00111111 | | | |
| 128	|	10000000 | | | | | 
| 192	|	11000000 | | | | |
| 224	|	11100000 | | | | |
| 240	|	11110000 | | | | |
| 0 - 127 | | U-00000000 - U-0000007F  | 0xxxxxxx | 1Byte, Most of the alphabet characters |
| 128 - 2047 | | U-00000080 - U-000007FF | 110xxxxx 10xxxxxx | 2Bytes for a chracter |
| 2048 - 65535	| | U-00000800 - U-0000FFFF | 1110xxxx 10xxxxxx 10xxxxxx | 3Bytes like Korean |
| 65536 - 2097151 | | U-00010000 - U-001FFFFF| 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx | 4Bytes |
| 2097152 - 67108863 | |  U-00200000 - U-03FFFFFF | 111110xx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx | 5Bytes |
| 67108864 - 2147483647 | | U-04000000 - U-7FFFFFFF | 1111110x 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx | 6Bytes |


* ex) c = 128, 128 = 10000000(in binary), put[0][1] -> `110`00010 `10`000000

