# Lo-Fi easy THM Machine
First-things-first, we have an default webpage on machine's IP:
![](Pasted%20image%2020250530203637.png)
Let's follow some links:

On the first `relax` link we can see a `/?page=relax.php` fragment.

Let's try replace `relax.php` with a relative path to `flag.txt`, that escalates current dir as high as possible ([CVE-2022-40734](https://nvd.nist.gov/vuln/detail/CVE-2022-40734)) and reads `flag.txt`
`http://10.10.236.179/?page=../../../../../../../../../../../../../../../flag.txt`
and this is a hit (flag content):
![](Pasted%20image%2020250530204621.png)