# docs


BIND
   
BIND, skrót od Berkeley Internet Name Domain, to najpopularniejszy serwer DNS używany w systemach Unix i Linux. Cechuje się stabilnością i elastycznością, co pozwala na konfigurację dowolnej liczby domen i subdomen. BIND posiada bogatą dokumentację, dzięki którym możliwe jest szybkie rozwikłanie jakichkolwiek problemów.

PowerDNS
   
PowerDNS jest innym popularnym wyborem do konfiguracji serwera DNS. Tak jak BIND, jest dostępny dla systemów Unix i Linux. PowerDNS pozwala na użycie różnych baz danych do przechowywania informacji o domenach.

Microsoft DNS 

Microsoft DNS to narzędzie dostępne na serwerach Windows Server. Jest łatwe w użytkowaniu dzięki narzędziom GUI i dobrze zintegrowane z innymi usługami Microsoft, takimi jak Active Directory.


Simple DNS Plus 
Jest to komercyjne narzędzie dostępne na platformę Windows, które pozwala na łatwe zarządzanie domenami i subdomenami dzięki interfejsowi użytkownika opartemu na GUI.


Pamiętaj, że wszystkie te serwery DNS muszą być skonfigurowane tak, aby router w Twojej lokalnej sieci kierował zapytania DNS do nich. Powinieneś także upewnić się, że masz zapasowy serwer DNS na wypadek, gdyby główny serwer przestał działać.




Aby ustawić dwie nazwy serwera (NS) - podstawowej i drugorzędnej - z PowerDNS, zacznij od zainstalowania PowerDNS na dwóch różnych serwerach, które będą pełnić te role.

Instalacja PowerDNS w systemie Debian/Ubuntu:

1. Aktualizuj repozytoria pakietów:
```
sudo apt-get update
```
2. Zainstaluj PowerDNS:
```
sudo apt-get install pdns-server pdns-backend-mysql
```
Podobne polecenia będą dla innych dystrybucji Linux, ale zamiast "apt-get" użyje się narzędzia do zarządzania pakietami tej dystrybucji (np. "yum" dla CentOS).

Potem przystąp do konfiguracji:

1. Na serwerze podstawowym, utwórz strefę DNS dla Twojej domeny i dodaj do niej rekord NS dla serwera drugorzędnego. Będzie wyglądać to więcej lub mniej tak (zależne od konkretnego GUI lub interfejsu API, którego używasz):
```
name: twojadomena.com
type: NS
content: ns2.twojadomena.com
```
Jak i również rekord NS dla serwera głównego:
```
name: twojadomena.com
type: NS
content: ns1.twojadomena.com
```
2. Na serwerze drugorzędnym, skonfiguruj PowerDNS do nasłuchiwania na zmiany w strefach DNS z serwera podstawowego. To także jest możliwe do zrobienia przez interfejs GUI lub API. W konfiguracji PowerDNS (pdns.conf) może to wyglądać tak:
```
slave=yes
master=ip_serwera_podstawowego
```
3. Ustal adresy IP dla ns1 i ns2:
```
name: ns1.twojadomena.com
type: A
content: ip_serwera_podstawowego
```
```
name: ns2.twojadomena.com
type: A
content: ip_serwera_drugorzędnego
```
W ten sposób, wszystkie zmiany dokonane na serwerze podstawowym będą automatycznie replikowane na serwerze drugorzędnym, zapewniając redundancję. Pamiętaj, że w praktyce, te dwa serwery powinny być najlepiej rozłożone geograficznie, aby zapewnić dostępność w przypadku awarii. 
