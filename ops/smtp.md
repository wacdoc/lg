# Zimba seva yo ey'okusindika mail ya SMTP

## ennyanjula

SMTP esobola okugula butereevu empeereza okuva mu batunda ebire, gamba nga:

* [Amazon SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali ekire email okusika](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Osobola n'okuzimba seva yo eya mail - okusindika okutaliiko kkomo, okutwalira awamu ssente ntono.

Wansi, tulaga mutendera ku mutendera engeri y’okuzimba seva yaffe eya mail.

## Okulonda seva

Seva ya SMTP eyeekyaza yeetaaga IP ey’olukale nga ports 25, 456, ne 587 ziggule.

Ebire eby’olukale ebikozesebwa ennyo bizibye emyalo gino mu butonde, era kiyinza okusoboka okugiggulawo ng’ofulumya ekiragiro ky’okukola, naye kizibu nnyo oluvannyuma lw’ebyo byonna.

Nkuteesa okugula okuva ku host erimu ports zino eziggule era nga ewagira okuteekawo amannya ga domain aga reverse.

Wano, nkuwa amagezi [Contabo](https://contabo.com) .

Contabo kkampuni ekola ku by’okukyaza abantu ng’esangibwa mu kibuga Munich ekya Girimaani, yatandikibwawo mu 2003 ng’emiwendo givuganya nnyo.

Bw’olonda Euro ng’ensimbi z’ogula, bbeeyi ejja kuba ya buseere (seva erimu memory ya 8GB ne CPU 4 egula yuan nga 529 buli mwaka, ate ssente z’okugiteeka mu kusooka za bwereere okumala omwaka gumu).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

Nga oteeka order, remark `prefer AMD` , era server erimu AMD CPU ejja kuba n'omutindo omulungi.

Mu bino wammanga, nja kutwala VPS ya Contabo ng’ekyokulabirako okulaga engeri y’okuzimba mail server yo.

## Ensengeka y'enkola ya Ubuntu

Enkola y’emirimu wano ye Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

Singa server ku ssh eraga `Welcome to TinyCore 13!` (nga bwe kiragibwa mu kifaananyi wansi), kitegeeza nti enkola eno tennateekebwawo. Nsaba okutuleko ssh olinde eddakiika ntono okuddamu okuyingira.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

`Welcome to Ubuntu 22.04.1 LTS` bw’efuluma, okutandika kuwedde, era osobola okugenda mu maaso n’emitendera gino wammanga.

### [Optional] Tandika embeera y'enkulaakulana

Omutendera guno gwa kwesalirawo.

Okusobola okwanguyiza, nateeka okuteeka n'okusengeka enkola ya software ya ubuntu mu [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Dukanya ekiragiro kino wammanga okuteeka n'okunyiga omulundi gumu.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Abakozesa Abachina, nsaba mukozese ekiragiro kino wammanga mu kifo ky'ekyo, era olulimi, ekitundu ky'essaawa, n'ebirala bijja kuteekebwawo mu ngeri ey'otoma.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo esobozesa IPV6

Ssobozesa IPV6 SMTP esobole nayo okuweereza emails ezirina endagiriro za IPV6.

okulongoosa `/etc/sysctl.conf`

Kyuusa oba yongera ku layini zino wammanga

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Goberera [n'okuyigiriza kwa contabo: Okwongera okuyungibwa kwa IPv6 ku seva yo](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Edit `/etc/netplan/01-netcfg.yaml` , yongera ku layini ntono nga bwe kiragibwa mu kifaananyi wansi (Contabo VPS default configuration file erina dda ennyiriri zino, just uncomment them).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Olwo `netplan apply` okufuula ensengeka ekyusiddwa okutandika okukola.

Oluvannyuma lw'okusengeka okutuuka ku buwanguzi, osobola okukozesa `curl 6.ipw.cn` okulaba endagiriro ya ipv6 ey'omukutu gwo ogw'ebweru.

## Clone etterekero ly'okusengeka ops

```
git clone https://github.com/wactax/ops.soft.git
```

## Tonda satifikeeti ya SSL ey'obwereere ku linnya lyo erya domain

Okusindika mail kyetaagisa satifikeeti ya SSL okusiba n'okussa omukono.

Tukozesa [acme.sh](https://github.com/acmesh-official/acme.sh) okukola satifikeeti.

acme.sh kye kimu ku bikozesebwa mu kussa omukono ku satifikeeti mu ngeri ey’obwengula, .

Yingiza sitoowa y'okusengeka ops.soft, dduka `./ssl.sh` , era folda ya `conf` ejja kutondebwa mu **dayirekita eya waggulu** .

Funa omuwa DNS wo okuva ku [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) , longoosa `conf/conf.sh` .

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Oluvannyuma dduka `./ssl.sh 123.com` okukola satifikeeti `123.com` ne `*.123.com` ez'erinnya lyo erya domain.

Okudduka okusooka kujja kuteeka [acme.sh mu](https://github.com/acmesh-official/acme.sh) ngeri ey’otoma era kwongerako omulimu ogutegekeddwa okuzza obuggya mu ngeri ey’otoma. Osobola okulaba `crontab -l` , waliwo layini nga eno wammanga.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Ekkubo lya satifikeeti ekoleddwa lye kintu nga `/mnt/www/.acme.sh/123.com_ecc。`

Okuzza obuggya satifikeeti kujja kuyita `conf/reload/123.com.sh` script, okulongoosa script eno, osobola okwongerako ebiragiro nga `nginx -s reload` okuzza obuggya cache ya satifikeeti y'enkola ezikwatagana.

## Zimba seva ya SMTP ne chasquid

[chasquid](https://github.com/albertito/chasquid) ye seva ya SMTP ey'enkozesa enzigule ewandiikiddwa mu lulimi Go.

Nga ekifo kya pulogulaamu za mail server ez’edda nga Postfix ne Sendmail, chasquid nnyangu era nnyangu okukozesa, era nnyangu n’okukulaakulanya okw’okubiri.

Run `./chasquid/init.sh 123.com` ejja kuteekebwawo mu ngeri ey’otoma nga onyiga omulundi gumu (kyusa 123.com n’erinnya lyo erya domain ly’osindika).

## Tegeka DKIM y'Omukono gwa Email

DKIM ekozesebwa okusindika emikono gya email okutangira ebbaluwa okutwalibwa nga spam.

Oluvannyuma lw'ekiragiro okutambula obulungi, ojja kusabibwa okuteekawo likodi ya DKIM (nga bwe kiragibwa wansi).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Just add a TXT record ku DNS yo (nga bwekiragibwa wansi).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Laba embeera y'empeereza & ebiwandiiko

 `systemctl status chasquid` Laba embeera y'empeereza.

Embeera y’okukola okwa bulijjo eri nga bwe kiragibwa mu kifaananyi wansi

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` oba `journalctl -xeu chasquid` esobola okulaba ekiwandiiko ky'ensobi.

## Okuzzaawo ensengeka y'erinnya ly'ekifo

Erinnya lya domain eridda emabega kwe kukkiriza endagiriro ya IP okugonjoolwa ku linnya lya domain erikwatagana.

Okuteekawo erinnya lya domain eridda emabega kiyinza okulemesa emails okumanyibwa nga spam.

Ebbaluwa bw’efunibwa, seva efuna ejja kukola okwekenneenya erinnya ly’ekifo eky’okudda emabega ku ndagiriro ya IP ya seva esindika okukakasa oba seva esindika erina erinnya ly’ekifo ekidda emabega ettuufu.

Singa seva esindika terina linnya lya domain eridda emabega oba singa erinnya lya domain eridda emabega terikwatagana na ndagiriro ya IP ya seva esindika, seva efuna eyinza okutegeera email nga spam oba okugigaana.

Kyalira [https://my.contabo.com/rdns](https://my.contabo.com/rdns) era osengeke nga bwe kiragibwa wansi

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Oluvannyuma lw'okuteeka erinnya ly'ekifo eky'emabega, jjukira okutegeka okusalawo okw'omu maaso okw'erinnya ly'ekifo ipv4 ne ipv6 ku seva.

## Edita erinnya ly'omukozi wa chasquid.conf

Kyuusa `conf/chasquid/chasquid.conf` ku muwendo gw'erinnya ly'ekifo eky'emabega.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Oluvannyuma dduka `systemctl restart chasquid` okuddamu okutandika empeereza.

## Backup conf okutuuka ku git repository

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Okugeza, nkola back up ya conf folder ku github process yange nga bweti

Sooka okole sitoowa ey’obwannannyini

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Yingiza conf directory oweereze mu sitoowa

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Okwongerako omusindika

okudduka

```
chasquid-util user-add i@wac.tax
```

Asobola okwongerako omusindika

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Kakasa nti ekigambo ky'okuyingira kiteekeddwa bulungi

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Oluvannyuma lw'okugattako omukozesa, `chasquid/domains/wac.tax/users` ejja kulongoosebwa, jjukira okugiweereza mu sitoowa.

## DNS yongera ku likodi ya SPF

SPF ( Sender Policy Framework ) ye tekinologiya ow’okukakasa email akozesebwa okutangira obufere bwa email.

Ekakasa omuntu atuweereza ebbaluwa ng’ekebera oba endagiriro ya IP y’oyo agiweereza ekwatagana ne DNS records z’erinnya ly’ekifo ly’egamba nti lye, ne kiremesa abafere okuweereza email ez’obulimba.

Okwongerako ebiwandiiko bya SPF kiyinza okulemesa email okuzuulibwa nga spam nga bwe kisoboka.

Singa domain name server yo tewagira kika kya SPF, just add TXT type record.

Okugeza, SPF ya `wac.tax` eri bweti

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF ku `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Weetegereze nti nnina `include:_spf.google.com` wano, kino kiri bwe kityo kubanga nja kutegeka `i@wac.tax` nga endagiriro y'okusindika mu Google mailbox oluvannyuma.

## Ensengeka ya DNS DMARC

DMARC kifupi kya (Okukakasa obubaka obusinziira ku kitundu, Okukola lipoota & Okukwatagana).

Kikozesebwa okukwata SPF bounces (oyinza okuba nga kiva ku nsobi z'okusengeka, oba omuntu omulala yeefuula ggwe okuweereza spam).

Okwongerako ekiwandiiko kya TXT `_dmarc` , .

Ebirimu biri bwe biti

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Amakulu ga buli paramita gali bwe gati

### p (Enkola) .

Kiraga engeri y'okukwatamu email eziremererwa okukakasa SPF (Sender Policy Framework) oba DKIM (DomainKeys Identified Mail). Parameter ya p esobola okuteekebwa ku emu ku miwendo esatu:

* tewali: Tewali kikolebwa, ebyava mu kukakasa byokka bye biddizibwa oyo abiweereza nga biyita mu nkola y’okutegeeza ku email.
* Kalantiini: Teeka mail etayise mu kukakasa mu folda ya spam, naye nga tegenda kugaana mail butereevu.
* reject: Gaana butereevu email ezilemererwa okukakasa.

### fo (Ebyokulonda mu kulemererwa) .

Laga omuwendo gw'amawulire agaddizibwa enkola y'okukola lipoota. Kiyinza okuteekebwa ku emu ku miwendo gino wammanga:

* 0: Lipoota ebivudde mu kukakasa obubaka bwonna
* 1: Loopa obubaka bwokka obulemererwa okukakasa
* d: Okuloopa kwokka okulemererwa okukakasa amannya ga domain
* s: okuloopa kwokka okulemererwa okukakasa SPF
* l: Loopa kwokka okulemererwa okukakasa DKIM

### rua & ruf

* rua (Okutegeeza URI ku lipoota ezikuŋŋaanyiziddwa): Endagiriro ya email ey’okufuna lipoota ezikuŋŋaanyiziddwa
* ruf (Okutegeeza URI ku lipoota za Forensic): endagiriro ya email okufuna lipoota enzijuvu

## Okwongerako ebiwandiiko bya MX okuweereza email ku Google Mail

Olw’okuba saasobola kufuna mailbox ya kkampuni ya bwereere ewagira endagiriro za bonna (Catch-All, esobola okufuna email zonna eziweerezeddwa ku linnya lino erya domain, awatali bukwakkulizo ku prefixes), nakozesa chasquid okuweereza email zonna mu mailbox yange eya Gmail.

**Bw’oba ​​olina mailbox yo eya bizinensi esasulwa, nsaba tokyusa MX era obuuke omutendera guno.**

Edita `conf/chasquid/domains/wac.tax/aliases` , teekawo ebbaluwa y'okuweereza

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` eraga email zonna, `i` ye email address prefix y'omukozesa asindika eyatondebwa waggulu. Okutambuza mail, buli mukozesa yeetaaga okwongerako layini.

Oluvannyuma yongerako likodi ya MX (nsonga butereevu ku ndagiriro y’erinnya ly’ekifo eky’emabega wano, nga bwe kiragibwa mu layini esooka mu kifaananyi wansi).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Oluvannyuma lw’okusengeka okuggwa, osobola okukozesa endagiriro za email endala okuweereza email ku `i@wac.tax` ne `any123@wac.tax` okulaba oba osobola okufuna email mu Gmail.

Bwe kitaba bwe kityo, kebera ekiwandiiko kya chasquid ( `grep chasquid /var/log/syslog` ).

## Weereza email ku i@wac.tax ng'okozesa Google Mail

Oluvannyuma lwa Google Mail okufuna mail, mu butonde nnalina essuubi okuddamu ne `i@wac.tax` mu kifo kya i.wac.tax@gmail.com.

Kyalira [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) n'onyiga ku "Yongera endagiriro ya email endala".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Oluvannyuma, ssaamu koodi y’okukakasa efunibwa email eyaweerezeddwa.

N'ekisembayo, kiyinza okuteekebwa nga endagiriro y'omusindika esookerwako (nga kw'otadde n'enkola okuddamu n'endagiriro y'emu).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Mu ngeri eno, tumalirizza okuteekawo SMTP mail server ate mu kiseera kye kimu tukozesa Google Mail okusindika n’okufuna email.

## Weereza email y'okugezesa okukebera oba ensengeka efunye obuwanguzi

Yingira mu `ops/chasquid`

Run `direnv allow` okuteeka ebisinziirwako (direnv etekeddwa mu nkola y'okutandikawo ekisumuluzo kimu emabega era hook eyongezeddwa ku kisusunku)

olwo odduke

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Amakulu ga paramita gali bwe gati

* omukozesa: erinnya ly'omukozesa SMTP
* okuyita: Ekigambo ky'okuyingira ekya SMTP
* eri: oyo afuna

Osobola okuweereza email y'okugezesa.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Kirungi okukozesa Gmail okufuna email ez'okugezesa okukebera oba ensengeka zituuse bulungi.

### Ensirifu ya mutindo gwa TLS

Nga bwe kiragibwa mu kifaananyi wansi, waliwo ekizibiti kino ekitono, ekitegeeza nti satifikeeti ya SSL ekoleddwa bulungi.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Oluvannyuma nyweza "Show Original Email".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Nga bwe kiragibwa mu kifaananyi wansi, omuko gwa Gmail ogwa mail ogwasooka gulaga DKIM, ekitegeeza nti ensengeka ya DKIM efunye obuwanguzi.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Kebera Received mu mutwe gwa email eyasooka, era osobola okulaba nti endagiriro y’omusindika ye IPV6, ekitegeeza nti IPV6 nayo etegekeddwa bulungi.
