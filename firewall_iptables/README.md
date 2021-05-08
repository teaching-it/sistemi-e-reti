# GNU/Linux firewall: iptables

Innanzi tutto, una breve ma efficace definizione (Wikipedia):

*iptables is a user-space utility program that allows a system administrator to configure the IP packet filter rules of the Linux kernel firewall, implemented as different Netfilter modules. The filters are organized in different tables, which contain chains of rules for how to treat network traffic packets. Different kernel modules and programs are currently used for different protocols; iptables applies to IPv4, ip6tables to IPv6, arptables to ARP, and ebtables to Ethernet frames.*

Dunque, iptables è un tool da command line che dialoga direttamente con Netfilter, la componente fondamentale del Kernel Linux che, a sua volta, offre numerose operazioni e funzioni di packet filtering e manipolazione del traffico di rete (ad esempio: NAPT, port-forwarding).

## Chains

iptables è strutturato in **chains**, di cui le 3 principali sono: input, forward, output.

1. Input: è utilizzata per il controllo delle connessioni in ingresso. Ad esempio: se un un client tenta di connettersi alla macchina via SSH, a seconda del comportamento definito iptables potrebbe decidere di accettare o scartare la richieta di connessione.

2. Forward: questa *chain* si occupa di filtrare il traffico di rete che non è direttamente destinato alla macchina. Ad esempio: è abbastanza comune che una macchina GNU/Linux venga impiegata come router. Spesso il traffico di rete *transita* dal router e più difficilmente è destinato al router stesso: il traffico di rete è dunque instradato verso la destinazione (o ad un altro nodo intermedio lungo il percorso).

3. Output: si occupa di filtrare le richieste di connessione in uscita. Ad esempio: se viene generato un semplice ping, iptables ispezionerà il contenuto del pacchetto e, a seconda delle regole definite, deciderà se lasciar transitare o meno la richiesta.

Per visualizzare lo stato delle *chain* iptables:

```root
root@volta:~# iptables -L -v
```

## Policy

Prima di procedere alla configurazione delle regole di firewalling, potrebbe essere conveniente definire il comportamento di default delle 3 *chains*.

Ovvero: che tipo di decisione deve intraprendere iptables, qualora per il pacchetto ispezionato non vi sia alcuna corrispondenza nell'elenco delle regole definite?

Il seguente comando consente di ispezionare la *default policy* per ciascuna *chain*:

```root
root@volta:~# iptables -L | grep policy
```

Com'è possibile intuire dall'output del comando, la policy di default, per ciascuna delle 3 *chain* è quella di accettare qualsiasi tipo di richiesta.

È pratica comune modificare la *default policy*:


```root
root@volta:~# iptables --policy INPUT DROP
root@volta:~# iptables --policy OUTPUT DROP
root@volta:~# iptables --policy FORWARD DROP
```

In questo modo verranno rifiutati tutti i tentativi di connessione, in ingresso, in uscita o in transito.

## Responses

Un altro aspetto fondamentale sono i comportamenti (le risposte) che è possibile applicare alle richieste di connessione.

1. Accept: acconsente la richiesta di connessione.
2. Drop: rifiuta "brutalmente" la richiesta.
3. Reject: non consente la connessione, e genera un "pacchetto di cortesia" (ICMP) in risposta.

### To be continued ... ###