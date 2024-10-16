# Client MQTT v5
## Introducere
MQTT v5 (Message Queuing Telemetry Transport) este un protocol de transport al mesajelor care se bazează pe TCP/IP și pe conceptul publish/subscribe și este destinat pentru conexiuni cu lățime de bandă redusă și latență scăzută. TCP este un protocol orientat pe conexiune, cu corectare a erorilor, și garantează că pachetele sunt primite în ordine. Majoritatea clienților MQTT se vor conecta la broker și vor rămâne conectați chiar și atunci când nu trimit date, iar conexiunile sunt recunoscute de broker printr-un mesaj de confirmare.
## MQTT v5 VS MQTT v3.1.1
MQTT 5 reprezintă o evoluție semnificativă a protocolului MQTT, fiind special adaptat pentru IoT. MQTT v5 adaugă îmbunătățiri în ceea ce privește garanțiile livrării mesajelor, prin mecanisme QoS (0, 1, 2), precum si opțiuni de control al fluxului de mesaje (Reason Codes, User Properties) si funcții avansate de gestionare a erorilor.
## Broker MQTT
Brokerul MQTT este un server care primește mesaje de la Publisher și le livrează abonaților în funcție de topicurile la care aceștia s-au abonat. Brokerul gestionează conexiunile clienților, abonările și dezabonările și asigură livrarea mesajelor conform nivelurilor specificate de QoS. Mesajele MQTT conțin un topic, iar brokerul folosește listele de abonamente pentru a determina care clienți vor primi mesajul. 
## Client MQTT
Clienții MQTT publică un mesaj către un broker MQTT, iar alți clienți MQTT se abonează la mesajele pe care doresc să le primească. Clienții MQTT pot fi publicatori, abonați sau amândouă si  nu trebuie să fie conectați simultan sau să fie informați despre existența celorlalți, deoarece comunicarea se realizeaza prin broker. Aceștia pot fi orice dispozitiv sau aplicație care poate stabili o conexiune cu broker-ul MQTT folosind protocolul MQTT, cum ar fi dispozitive IoT, aplicații mobile sau alte servere.
## Publisher/Subscriber
MQTT Publisher generează și trimite mesaje către un broker MQTT, care le primește si le distribuie către clienții abonați la un anumit topic.  Publisherul unui mesaj de aplicație se așteaptă, în mod normal, ca serverele să redirecționeze mesajul către subscriberi și ca acești abonați să fie capabili să proceseze mesajele.
MQTT Subscriber se abonează la topicuri de pe un broker MQTT pentru a citi mesaje de la broker. Aceasta funcționează ca un client MQTT care primește mesaje, generând o înregistrare pentru fiecare mesaj. Atunci când configurezi originea, specifici informațiile necesare pentru a te conecta la brokerul MQTT.  În cazul în care brokerul solicită autentificare, trebuie să definești numele de utilizator și parola.
![Publisher/Subscriber](https://imgur.com/ymEqRhx.png)
## Topicuri
Topicurile sunt șiruri ierarhice care definesc subiectul sau categoria unui mesaj. Atunci când publicatorii trimit mesaje către broker, aceștia le asociază cu un topic specific. Abonații își exprimă interesul de a primi mesaje prin abonarea la unul sau mai multe topicuri MQTT. Broker-ul rutează apoi mesajele către abonații corespunzători pe baza abonațiilor lor la topicuri.
De exemplu, un topic ierarhic ar putea fi:
dispozitive/senzori/temperatura












