# PMTiles v3归档范围

127字节头数字均小端；内部compression枚举1=none、2=gzip；tile compression另记，不从文件扩展名猜。root offset绝对，leaf条目offset相对leaf_directory_offset，tile条目offset相对tile_data_offset。目录四数组顺序为tileID差分、run_length、length、offset编码，长度为entry_count，u64 varint至多10字节且最高越界拒绝。offset0只可从前条已解offset+length继承，第一条不允许0。

tile_id为先前zoom的4^z个数和，加当前zoom内标准PMTiles Hilbert索引；z0编号0，z1依次(0,0)=1、(0,1)=2、(1,1)=3、(1,0)=4。支持z<=26，不执行地理坐标转换。叶指针正常仅指leaf条目，leaf中再次出现目录指针是范围外异常，需阻断而非递归猜格式；预算与已访问范围仍用于识别循环/重复引用。run覆盖连续tile_id而不是坐标矩形。

正常数据id0指AAA、id1/2共享BBB、id5指CCC，id3/4为空洞。header计数分别为addressed=4、entries=3、contents=3。tile内容为不透明三字节演示片段，不声称合法MVT。采集成功只说明容器引用和字节身份正确。初始工具不解目录或Hilbert。
