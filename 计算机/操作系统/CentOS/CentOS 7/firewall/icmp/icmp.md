# icmp

禁止ping

```bash
firewall-cmd --permanent --add-rich-rule='rule protocol value=icmp drop'
firewall-cmd --permanent --remove-rich-rule='rule protocol value=icmp drop'
```
