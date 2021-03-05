# CA证书
创建 证书
puppetserver ca setup  --ca-name zhiweidata --certname puppet.zhiweireach.com

手动签名
puppetserver ca sign --certname NAME

手动签名全部
puppetserver ca sign --all

查看所有证书
puppetserver ca list --all