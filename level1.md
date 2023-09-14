## level1
* <font color="red">InterfaceA1</font>と<font color="blue">InterfaceB1</font>のサブネットマスクが同一のため、IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="red">InterfaceA1</font>に設定する。

```plantuml
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
