# tlwe2021-krypto-2

tech:lounge Winter Edition 2021 // Verschlüsselung: RSA, digitale Signaturen & Co. einsetzen

## Primzahlen

Primzahl = natürliche Zahl > 1, die nur durch 1 und sich selbst teilbar ist
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, …

Behauptung: Es gibt unendlich viele Primzahlen
Vorgehen  : Beweis durch Widerspruch
Beweis    : Angenommen, es gäbe endlich viele Primzahlen
            => Es gibt eine größte Primzahl p
            => Wir konstruieren eine Zahl q = (2 * 3 * 5 * ... * p) + 1
            => q > p => q kann keine Primzahl sein
            => Es muss eine Primfaktorzerlegung von q geben
            => Man findet keine => q muss eine Primzahl sein
            => Das ist ein Widerspruch zur Annahme, dass p die größte Primzahl ist
            => Es muss unendlich viele Primzahlen geben => Behauptung bewiesen

Ist p eine Primzahl?
=> Naiver Ansatz: Alle Zahlen von 2 bis (p-1) als Teiler ausprobieren

## Eulersche Phi-Funktion

phi(n) = Wie viele Zahlen gibt es zwischen 1 und n, die keinen gemeinsamen Teiler mit n haben

1  => phi(1)  => 1
6  => phi(6)  => 1 und 5 => 2
13 => phi(13) => 12

phi(15) = phi(5 * 3) = phi(5) * phi(3) = 8

## Primfaktorzerlegung

 8 = 2 * 2 * 2
 9 = 3 * 3
10 = 2 * 5
12 = 2 * 2 * 3

61 * 79 = ?
=> 4819

6787 = ? * ?
=> 11 * 617 (durch ausprobieren)

"Falltürfunktion"

## Restklassen

N = { 1, 2, 3, 4, 5, 6, ... }
Z = { ..., -3, -2, -1, 0, 1, 2, 3, ... }
Z/12 = { 0, 1, 2, ..., 10, 11 }
Z/n = { 0, 1, 2, ..., n-2, n-1 }

7 + 9 mod 12 = 16 mod 12 = 4

x^a^b = x^b^a
2^3^4 = 4096 = 2^4^3

((x^a mod y)^b mod y) = ((x^b mod y)^a mod y)

## RSA (Rivest, Shamir, Adleman)

0b1100101 = 64 + 32 + 4 + 1 = 101

1. Schritt: Zwei geheime Primzahlen suchen, p und q
2. Schritt: Den Modul N = p * q berechnen
3. Schritt: phi(N) = phi(p * q) = phi(p) * phi(q) = (p - 1) * (q - 1)
4. Schritt: e (zufällig gewählt, 1 < e < phi(N), e teilerfremd zu phi(N))
5. Schritt: d = e * d mod phi(N) = 1

M: Message
C: Encrypted Message
e: Verschlüsselungsschlüssel
d: Entschlüsselungsschlüssel
N: Modul

C = M ^ e mod N
M = C ^ d mod N

=> M = (M ^ e mod N) ^ d mod N
   M = (M ^ d mod N) ^ e mod N

^e^d => muss sich gegenseitig aufheben in mod N

Alice möchte eine Nachricht M an Bob schicken
Zum Verschlüsseln braucht Alice N und e
Zum Entschlüsseln braucht Bob N und d

Ablauf:
- Alice möchte Nachricht an Bob schicken
- Bob erzeugt N, e und d
  - (N, e) => öffentlich (public key)
  - (N, d) => privat (private key)

Rechenbeispiel:

p = 3, q = 5
N = 3 * 5 = 15
phi(N) = 2 * 4 = 8
e = 3

d * e mod phi(N) = 1
d * 3 mod 8 = 1
=> d = 3

M = 7
Verschlüsseln: C = M ^ e mod N = 7 ^ 3 mod 15 = 343 mod 15 = 13
Entschlüsseln: M = C ^ d mod N = 13 ^ 3 mod 15 = 2197 mod 15 = 7

## Vor- und Nachteile

- Vorteile
  - Kein geheimer Schlüsselaustausch mehr erforderlich
  - Viel geringere Anzahl an Schlüsseln als bei symmetrischen Verfahren
  - Mathematisch vermutlich sicher (hängt von der Sicherheit der Falltürfunktion ab)
- Nachteile
  - Relativ langsam
  - Nachrichtenlänge begrenzt

## In der Praxis: Hybridverfahren

Alice will eine Nachricht M an Bob versenden
Alice denkt sich einen Zufallsschlüssel r aus
Alice verschlüsselt M mit r (256 Bit) mit AES-256-CBC => C
Alice verschlüsselt r mit dem öffentlichen Schlüssel (N, e) von Bob => r'
Alice sendet C und r' an Bob
Bob entschlüsselt r' mit seinem privaten Schlüssel (N, d) => r
Bob entschlüsselt C mit r => M

## Elliptische Kurve (ECC = Elliptic Curve Cryptography)

Lineare Gleichung     : y = mx + n
Quadratische Gleichung: y = x^2 + ax + b

y^2 = x^3 + ax + b

## Digitale Signaturen

Alice sendet eine Nachricht an Bob:

- verschlüsseln(Nachricht, pubKey(Bob))) <--- kann jede:r machen
- entschlüsseln(Nachricht, privKey(Bob)) <--- kann nur Bob machen

Alice möchte eine Nachricht an Bob signieren:

- verschlüsseln(Nachricht, privKey(Alice)) <--- kann nur Alice machen
- entschlüsseln(Nachricht, pubKey(Alice))  <--- kann jeder machen

Beides zusammen:

Alice möchte eine Nachricht an Bob senden und signieren:

- verschlüsseln(verschlüsseln(Nachricht, privKey(Alice)), pubKey(Bob))
- entschlüsseln(entschlüsseln(Nachricht, privKey(Bob)), pubKey(Alice))

## Integrität von Nachrichten

- Einwegfunktionen
  - `'Hallo Welt'.length` => 10

- Hashfunktionen
  - f(input) => hash
  - Einwegfunktionen
  - Hashes haben eine feste Länge
  - Kollisionsfrei: f(a) != f(b) => a != b

- MD (Message Digest)
  - MD1 (veraltet)
  - MD4 (veraltet)
  - MD5 (veraltet)
- SHA (Secure Hash Algorithm)
  - SHA1 (veraltet)
  - SHA2
    - SHA256 <---
    - SHA384
    - SHA512 <---
  - SHA3 (Zukunft)

Alice:
- Hallo Welt, 2d2da19605a34e037dbe82173f98a992a530a5fdd53dad882f570d4ba204ef30
- 2d2da19605a34e037dbe82173f98a992a530a5fdd53dad882f570d4ba204ef30 => OK

## Message Authentication Codes (MAC)

Alice und Bob: Gemeinsamen geheimen Schlüssel S
Alice sendet:
- Hallo Welt, hash(S + 'Hallo Welt')

=> Hash-basierte MACs => HMACs
