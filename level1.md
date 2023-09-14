# level1

## Goal1
* <font color="red">InterfaceA1</font>と<font color="blue">InterfaceB1</font>のサブネットマスクが同一。
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="red">InterfaceA1</font>に設定する。

## Goal2
* <font color="red">InterfaceC1</font>と<font color="blue">InterfaceD1</font>のサブネットマスクが同一。
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="red">InterfaceD1</font>に設定する。

## chart
```mermaid
flowchart
subgraph Goal2
    direction BT
    ClientC1-->InterfaceC1--InterafaceC1.NW == InterafaceD1.NW <br>AND<br>InterafaceC1.IP != InterafaceD1.IP-->InterfaceD1-->ClientD1
end
subgraph Goal1
    direction BT
    ClientA1-->InterfaceA1--InterafaceA1.NW == InterafaceB1.NW <br>AND<br>InterafaceA1.IP != InterafaceB1.IP-->InterfaceB1-->ClientB1
end
```
## example
```mermaid
flowchart
subgraph Goal2
    direction BT
    MyMac-->IP:211.191.109.75,MASK:255.255.0.0-- InterafaceC1.NW == InterafaceD1.NW <br>AND<br>InterafaceC1.IP != InterafaceD1.IP-->IP:211.191.109.76,MASK:255.255.0.0-->MyLittleSister'sComputer
end
subgraph Goal1
    direction BT
    MyPC-->IP:104.96.23.13,MASK:255.255.255.0-- InterafaceA1.NW == InterafaceB1.NW <br>AND<br>InterafaceA1.IP != InterafaceB1.IP-->IP:104.96.23.12,MASK:255.255.255.0-->MyLittleBrother'sComputer
end
```
