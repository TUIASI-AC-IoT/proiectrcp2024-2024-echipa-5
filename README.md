# Client MQTT v5
## Introducere
**MQTT v5** (Message Queuing Telemetry Transport) este un protocol de transport al mesajelor care se bazează pe TCP/IP și pe conceptul publish/subscribe și este destinat pentru conexiuni cu lățime de bandă redusă și latență scăzută. TCP este un protocol orientat pe conexiune, cu corectare a erorilor, și garantează că pachetele sunt primite în ordine. Majoritatea clienților MQTT se vor conecta la broker și vor rămâne conectați chiar și atunci când nu trimit date, iar conexiunile sunt recunoscute de broker printr-un mesaj de confirmare.
## MQTT v5 VS MQTT v3.1.1
**MQTT v5** reprezintă o evoluție semnificativă a protocolului MQTT, fiind special adaptat pentru IoT. MQTT v5 adaugă îmbunătățiri în ceea ce privește garanțiile livrării mesajelor, prin mecanisme QoS (0, 1, 2), precum si opțiuni de control al fluxului de mesaje (Reason Codes, User Properties) si funcții avansate de gestionare a erorilor.
## Broker MQTT
**Brokerul MQTT** este un server care primește mesaje de la Publisher și le livrează abonaților în funcție de topicurile la care aceștia s-au abonat. Brokerul gestionează conexiunile clienților, abonările și dezabonările și asigură livrarea mesajelor conform nivelurilor specificate de QoS. Mesajele MQTT conțin un topic, iar brokerul folosește listele de abonamente pentru a determina care clienți vor primi mesajul. 
## Client MQTT
**Clienții MQTT** publică un mesaj către un broker MQTT, iar alți clienți MQTT se abonează la mesajele pe care doresc să le primească. Clienții MQTT pot fi publicatori, abonați sau amândouă si  nu trebuie să fie conectați simultan sau să fie informați despre existența celorlalți, deoarece comunicarea se realizeaza prin broker. Aceștia pot fi orice dispozitiv sau aplicație care poate stabili o conexiune cu broker-ul MQTT folosind protocolul MQTT, cum ar fi dispozitive IoT, aplicații mobile sau alte servere.
## Publisher/Subscriber
**MQTT Publisher** generează și trimite mesaje către un broker MQTT, care le primește si le distribuie către clienții abonați la un anumit topic.  Publisherul unui mesaj de aplicație se așteaptă, în mod normal, ca serverele să redirecționeze mesajul către subscriberi și ca acești abonați să fie capabili să proceseze mesajele.

**MQTT Subscriber** se abonează la topicuri de pe un broker MQTT pentru a citi mesaje de la broker. Aceasta funcționează ca un client MQTT care primește mesaje, generând o înregistrare pentru fiecare mesaj. Atunci când configurezi originea, specifici informațiile necesare pentru a te conecta la brokerul MQTT.  În cazul în care brokerul solicită autentificare, trebuie să definești numele de utilizator și parola.
![Publisher/Subscriber](https://imgur.com/ymEqRhx.png)
## Topicuri
**Topicurile** sunt șiruri ierarhice care definesc subiectul sau categoria unui mesaj. Atunci când publicatorii trimit mesaje către broker, aceștia le asociază cu un topic specific. Abonații își exprimă interesul de a primi mesaje prin abonarea la unul sau mai multe topicuri MQTT. Broker-ul rutează apoi mesajele către abonații corespunzători pe baza abonațiilor lor la topicuri.

De exemplu, un topic ierarhic ar putea fi: dispozitive/senzori/temperatura

## LastWill
**Configurare Last Will**: Un client poate seta un mesaj "Last Will" care va fi publicat de broker către ceilalți clienți în cazul în care clientul se deconectează brusc sau pierde conexiunea. Ideea mesajului de tip Last Will este de a notifica un abonat că publisherul nu este disponibil din cauza unei întreruperi a rețelei. Acesta este setat de clientul publicator și este stabilit pe baza fiecărui topic, ceea ce înseamnă că fiecare topic poate avea propriul mesaj de Last Will. Mesajul este stocat pe broker și este trimis oricărui client abonat (la acel topic) dacă conexiunea cu publisherul eșuează. Dacă publisherul se deconectează normal, mesajul de Last Will nu este trimis.

## Autenficare cu utilizator cu parola

**Autentificarea** face parte din securitatea la nivel de transport și aplicație în MQTT. Dacă se folosesc câmpurile de utilizator și parolă din pachetul MQTT CONNECT pentru mecanismele de autentificare și autorizare, utilizarea TLS asigură o securitate sporită (deși MQTT se bazează pe protocolul de transport TCP, care nu folosește comunicarea criptată).

## Mecanisme 
# KeepAlive
**Mecanismul Keep Alive** în MQTT asigură funcționarea conexiunii și oferă o modalitate pentru broker de a detecta dacă un client devine nefuncțional sau se deconectează. 

**Keep Alive** este un întreg de două byte-uri care reprezintă un interval de timp măsurat în secunde. Acesta este timpul maxim permis care poate trece între momentul în care clientul finalizează transmiterea unui pachet de control MQTT și momentul în care începe să-l trimită pe următorul. Este responsabilitatea Clientului să se asigure că intervalul dintre pachetele de control MQTT trimise nu depășește valoarea Keep Alive. 

Dacă valoarea Keep Alive este diferită de zero și serverul nu primește un pachet de control MQTT de la Client în termen de o dată și jumătate din perioada de timp Keep Alive, acesta trebuie să închidă conexiunea de rețea cu Clientul ca și cum rețeaua ar fi eșuat. O valoare Keep Alive de 0 are efectul de a dezactiva mecanismul Keep Alive. 

Clientul trebuie să trimită un pachet PINGREQ către broker cel puțin o dată în cadrul acestui interval de timp pentru a indica prezența sa și a menține conexiunea activă. La primirea unui pachet PINGREQ, brokerul răspunde cu un pachet PINGRESP, confirmând că conexiunea este încă activă.

# Quality of Service
MQTT livrează mesaje de aplicație conform nivelurilor de Calitate a Serviciului (**QoS**). Protocolul de livrare gestionează mesajele între un singur expeditor și un singur receptor. Când serverul trimite mesaje către mai mulți clienți, fiecare este gestionat independent.

**QoS 0: livrare cel mult o dată**
La cel mai mic nivel, **QoS 0** în MQTT oferă “a best-effort delivery mechanism”, unde expeditorul nu așteaptă o confirmare sau o garanție a livrării mesajului. Astfel, destinatarul nu confirmă primirea mesajului, iar expeditorul nu îl stochează sau retransmite. QoS 0, denumit în mod obișnuit “fire and forget”, funcționează asemănător cu protocolul TCP subiacent, unde mesajul este trimis fără confirmări suplimentare.
![QoS_0](https://imgur.com/HvwXm9Q.png)
**QoS 1: livrare cel puțin o dată**
În **QoS 1** al MQTT, accentul este pus pe asigurarea livrării mesajului cel puțin o dată către receptor. Atunci când un mesaj este publicat cu QoS 1, expeditorul păstrează o copie a mesajului până când primește un pachet PUBACK de la receptor, confirmând primirea cu succes. Dacă expeditorul nu primește pachetul PUBACK într-un interval de timp rezonabil, retransmite mesajul pentru a asigura livrarea acestuia.
![QoS_1](https://imgur.com/1jKu3Tz.png)
**QoS 2: livrare exact o dată**
**QoS 2** oferă cel mai înalt nivel de serviciu în MQTT, asigurându-se că fiecare mesaj este livrat exact o dată destinatarilor intenționați. Pentru a atinge acest lucru, QoS 2 implică un schimb de mesaje în patru etape între expeditor și receptor.
![QoS_2](https://imgur.com/NXqrI9p.png)
Când un receptor primește un pachet PUBLISH cu QoS 2 de la un expeditor, acesta procesează mesajul publicat corespunzător și răspunde expeditorului cu un pachet PUBREC care confirmă primirea pachetului PUBLISH. Dacă expeditorul nu primește un pachet PUBREC de la receptor, acesta trimite din nou pachetul PUBLISH cu un flag de duplicat (DUP) până când primește o confirmare.
























