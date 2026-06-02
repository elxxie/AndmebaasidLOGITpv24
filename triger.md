## Trigger - päästik
### SQL triggerid on spetsiaalsed andmebaasi objektid, mis käivituvad automaatselt, kui toimub teatud sündmus (nt INSERT, UPDATE või DELETE).

--Trigger lisatud kirjeid jälgimiseks tabelis “linnad” – INSERT
--Jälgib andmete sisestamine tabelis linnad ja teeb vastava kirje tabelis logi
//	
CREATE TRIGGER linnaLisamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(aeg, toiming, andmed)
SELECT
GETDATE(),  --aeg
'on tehtud INSERT käsk',  --toiming
inserted.linnanimi  --andmed
FROM inserted;
//

<img width="790" height="402" alt="{E0B7A8EE-FA97-436D-B256-B18277132AAF}" src="https://github.com/user-attachments/assets/5d7395b7-b661-4e23-b2d5-57abd953eba1" />


--triger DELETE
//
CREATE TRIGGER linnaKustutamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on sisse logitud serverisse
'on tehtud DELETE käsk',  --toiming
concat('linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv)  --andmed
FROM deleted;

--drop triger ....
disable trigger linnaKustutamine on linnad;
enable trigger linnaKustutamine on linnad;

--DELETE trigeri kontroll
delete from linnad where linnID=3;

select * from linnad
select * from logi
//

<img width="610" height="591" alt="{E1DB975B-0532-4213-BF1B-BEB24166BB49}" src="https://github.com/user-attachments/assets/8d08cd15-6971-428a-8a98-b28bf587bf68" />
