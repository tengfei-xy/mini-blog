# 从http获取

file { 'C:/ProgramData/PuppetLabs/puppet/etc/puppet.conf':
ensure =>present,
source => "[http://adserver.monster.com:8088/test/puppet.conf](http://adserver.monster.com:8088/test/puppet.conf "http://adserver.monster.com:8088/test/puppet.conf")",
}
