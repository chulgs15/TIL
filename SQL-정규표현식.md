* https://stackoverflow.com/questions/41015852/parsing-select-statement-with-regex-for-custom-sql-parser

  * ```
    select\s+(.*?)\s*from\s+(.*?)\s*(where\s(.*?)\s*)?;
    ```
  * 일반적인 select 확인 .

* ```
  from\s+(.*?)\s*(where\s(.*?)\s*)
  ```

  * FROM ~ WHERE 까지 찾을 수 있음.

* http://jsqlparser.sourceforge.net/example.php
  * 준수한데?



- [x] 간단한거 부터 해보자. REGEXP 연속.
- [ ] https://regexr.com/ 여기들어가서 돌려.

```
/((CLN|EDR|PRP|AMW|XLE|ASN|MST|FUN|GCS|XDO|ZX|LNS|IA|FPA|ZPB|IZU|BEM|BQT|BFBS|BVAN|BTR|EAU|EMRP|EFA|EBOM|ECST|EGL|EINV|EOE|EAP|EPA|EPO|EQA|EAR|EWIP|ESVC|EARP|BUAS|BAR|EDCM|EPI|BMT|EOFA|EMAIL|EIV|XXETF|JMF|ITA|FTP|PFT|GMO|DNA|IPM|IBW|FV|ASL|EHM|EKPI|XXSM|XXCMN|APPLSYS|ALR|AX|AK|XLA|GL|RG|FA|HR|SSP|BEN|HXT|OTA|QA|ICX|AZ|BIS|PN|HXC|RLM|VEA|POM|FRM|BSC|FEM|AP|AR|OE|OSM|PA|CN|MFG|INV|PO|BOM|ENG|MRP|CRP|WIP|CZ|PJM|FLM|MSC|XTR|CS|CE|EC|JG|JE|JA|JL|GMA|GMD|GME|GMF|GMI|GML|GMP|GR|PMI|CUS|CUI|CUP|JTF|BIX|IEO|OKC|OKS|CSC|BIC|CSD|ASF|CSF|AMS|AMV|BIM|XNP|XDP|BIL|IES|AST|CCT|IBP|IBY|IBE|IBU|FII|HRI|ISC|OPI|POA|MSO|ONT|QP|WSH|MSD|WMS|WPS|CUF|CUA|IPA|ASG|IEX|OKX|ASO|CSP|OZF|IEU|IEM|OKE|ECX|GMS|IGW|PSB|PSP|CSR|IEB|IGF|IGS|WSM|MWA|IGC|PSA|APPLSYSPUB|APPS|PV|POS|ASP|BIV|CSI|CSL|CUG|EIMP|EAM|FTE|IGI|ITG|MSR|ENI|ODM|OKI|IEC|CSE|JTS|JTM|AHL|IMC|BNE|QRM|PON|OKL|IBC|QOT|CSM|DOM|EGO|DDD|PJI|EDWREP|CTXSYS|PORTAL30_SSO|PORTAL30|XNB|ZFA|ZSA))[.]/gi
```



* 방향성을 잘 잡아야해.
  * FROM 절을 LIST 로 보여준다. => 스키마. (개인이 찾는방법)
    * java 파일이 필요해.
  * 스키마. 을 찾아준다.
    * 이러면 스탠다드 스키라의 정규 표현식을 줘도 돼.



* ResultSetMetaData 로?
  * 속도 문제는 어때?
  * 찾아야 할 소스 Type 이 다르면 어때?
    * 왜냐하면 Select 파일이 xml 일 수도 있고, C# 일 수도 있고.
  * 그러면 하나의 소스로 결정해.
