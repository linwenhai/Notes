```bash
#!/bin/bash
num=1
while (( $num <= 128 ))
do
        echo sfexpay$num
        mysql -h sfsypay$num-m.db.sfdc.com.cn -uroot -p123456 -e "select id from sfexpay$num.tb_sf_pay_req;"
        num=$[$num + 1]
done
```

