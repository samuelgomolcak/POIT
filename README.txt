Spojazdnenie riešenia pre WebSocket komunikáciu

    O. Stiahnutie zdrojových súborov

        1. Priečinok TornadoWebSocketServerWithWebMobileClient umiestnite podľa ľubovôle na svoju VM.

    I. Inštalácia servera (server sluzi pre mobilneho web klienta aj pre Unity)

        1. Nainštaluje framework Tornado do svojej VM pomocou príkazu:
            $sudo pip install tornado

    II. Generovanie vlastného SSL certifikátu

        1. OpenSSL by v Raspbiane mal byť už nainštalovaný

        2. Vytvorte certifikát na svojom PC podľa návodu :
            https://blog.miguelgrinberg.com/post/running-your-flask-application-over-https - časť Self-Signed Certificates

            openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365 
	    * pri vytváraní certifikatu v poli "Common Name" treba zadať IP servera.
        
        3. Súbor crt.pem a key.pem umiestnite do priečinku TornadoWebSocketServerWithWebMobileClient na vašej VM.
        4. V tom istom priečinku sa nachádza súbor app.py. Ak nie su, doplnte v ňom názvy vygenerovaných súborov cert.pem a key.pem.

    III. Používanie na strane klienta

        A. Android, iOS, (aj overenie funkcnosti strany servera pre Unity)

            1. Choďte na stránku (je nutné ručne napísať "https://"):
                https://<IP_ADDRESS>:<PORT>
		* port je 8080
            2. Pomocou UI sa pripojte na server a posielajte dáta.

        B. Unity

            1. Do priečinku Assets vo svojom projekte vložte obsah priečinku UnityWebSocketClient_trebaBuildpreVasuIP.
            2. Skript WebSocketClient.cs použite na odosielanie dát. Po pripojení skriptu na objekt zadajte url:
                wss://<IP_ADDRESS>:<PORT>/stream

            UPOZORNENIE: Aby ste sa mohli pripojiť z Unity na server, je potrebné urobiť nasledovné:

                1. Na svojej VM choďte do adresára /etc/ssl/ a zmeňte vlastnosti súboru openssl.cnf tak, aby ste ho mohli prepisovať:
			$sudo chmod 0777 openssl.cnf
                2. V súbore vyhľadajte premennú "MinProtocol", zmeňte jej hodnotu na "TLSv1" a uložte súbor.