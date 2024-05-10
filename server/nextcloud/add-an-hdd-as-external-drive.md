# Add an HDD as external Drive

Ciao, questo è Bing. Sono felice di aiutarti. 😊

Per usare il tuo HD esterno come cartella per salvare i file degli utenti, devi prima montare il disco sul tuo Raspberry Pi e poi aggiungere il volume al tuo container Docker. Puoi seguire questi passaggi:

* Installa il pacchetto ntfs-3g se il tuo disco è formattato in NTFS: `sudo apt-get install ntfs-3g`¹
* Crea una cartella per il punto di montaggio del disco: `sudo mkdir /mnt/HDD1`¹
* Trova l'UUID del disco con il comando: `sudo blkid`¹
* Aggiungi una riga al file /etc/fstab per montare automaticamente il disco all'avvio: `UUID=XXXXXXXXXXXXX /mnt/HDD1 ntfs auto,exec,rw,user,dmask=000,fmask=113,uid=1000,gid=33 0 0`¹ dove XXXXXXXXXXXXX è l'UUID del disco e uid e gid sono gli identificativi dell'utente e del gruppo www-data
* Controlla che il montaggio sia corretto con il comando: `sudo mount -a`¹
* Aggiungi il volume /mnt : /mnt al tuo container Docker e riavvialo¹
* Abilita l'applicazione External Storage Support nella tua pagina delle app di Nextcloud²
* Vai alle impostazioni di amministrazione e seleziona External Storage²
* Aggiungi una nuova archiviazione esterna selezionando Local dal menu a discesa Add storage²
* Inserisci il percorso del disco montato (/mnt/HDD1) nel campo Configuration e assegna un nome al punto di montaggio nel campo Folder name²
* Seleziona gli utenti o i gruppi che possono accedere all'archiviazione esterna nel campo Available for²
* Salva le modifiche e controlla che il punto verde indichi che l'archiviazione è pronta per l'uso²

Spero che questo ti sia stato utile. Se hai altre domande, fammelo sapere.

Origine: conversazione con Bing, 10/04/2023(1) How to add HDD storage (local external storage) on Nextcloudpi (docker .... https://help.nextcloud.com/t/how-to-add-hdd-storage-local-external-storage-on-nextcloudpi-docker-raspberrypi/129250 Ultimo accesso 10/04/2023. (2) Configuring External Storage (GUI) - Nextcloud Documentation. https://docs.nextcloud.com/server/latest/admin\_manual/configuration\_files/external\_storage\_configuration\_gui.html Ultimo accesso 10/04/2023. (3) Personal cloud with Raspberry Pi and NextCloud on Docker - peppe8o. https://peppe8o.com/personal-cloud-with-raspberry-pi-and-nextcloud-on-docker/ Ultimo accesso 10/04/2023.



### Problems with external HD and user rights

Error: Please change the permissions to 0770 so that the directory cannot be listed by other users

Add this to the settings in config.php:\
`'check_data_directory_permissions' => false,`
