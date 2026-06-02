## Trigger - päästik

[Põhimõisted](README.md) | [Kasutajad](kasutaja.md) | [ Trigerid](triger.md) | [Protseduurid](protseaduurid.md) | [Sales](sales.md) | [Võtmed/Keys](kodutoo)

### SQL triggerid on spetsiaalsed andmebaasi objektid, mis käivituvad automaatselt, kui toimub teatud sündmus (nt INSERT, UPDATE või DELETE).

--Trigger lisatud kirjeid jälgimiseks tabelis “linnad” – INSERT
--Jälgib andmete sisestamine tabelis linnad ja teeb vastava kirje tabelis logi
///
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
///

<img width="790" height="402" alt="{E0B7A8EE-FA97-436D-B256-B18277132AAF}" src="https://github.com/user-attachments/assets/5d7395b7-b661-4e23-b2d5-57abd953eba1" />


--triger DELETE
///
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
///

<img width="610" height="591" alt="{E1DB975B-0532-4213-BF1B-BEB24166BB49}" src="https://github.com/user-attachments/assets/8d08cd15-6971-428a-8a98-b28bf587bf68" />

///
--Kombineerimine insert ja delete

disable trigger linnaKustutamine on linnad;
disable trigger linnaLisamine on linnad;

CREATE TRIGGER linnaLisamineKustutamine
ON linnad
AFTER INSERT, DELETE
AS
BEGIN
    SET NOCOUNT ON;

    INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)
    SELECT
        GETDATE(),
        SYSTEM_USER,
        'on tehtud INSERT käsk',
        CONCAT('linn: ', linnanimi, ', rahvaarv: ', rahvaarv)
    FROM inserted

    UNION ALL

    SELECT
        GETDATE(),
        SYSTEM_USER,
        'on tehtud DELETE käsk',
        CONCAT('linn: ', linnanimi, ', rahvaarv: ', rahvaarv)
    FROM deleted;
END;

insert into linnad(linnanimi, rahvaarv)
values 
('Narva2', 15633)

select * from linnad
select * from logi
///

<img width="587" height="765" alt="{75407A6C-0D1A-499E-B5A3-ADFCB3AE4210}" src="https://github.com/user-attachments/assets/50cbbe35-2030-48bb-9a82-cf121afc8297" />

///
--update triger
CREATE TRIGGER linnaUuendamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on sisse logitud serverisse
'on tehtud DELETE käsk',  --toiming
concat('vanad andmed - linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv,
' uued andmed -linn: ', inserted.linnanimi, ', rahvaarv -', inserted.rahvaarv)  --andmed
FROM deleted inner join inserted 
on deleted.linnID=inserted.linnID;

--update kontroll
update linnad set linnanimi='Narva22', rahvaarv=0 where linnanimi='Narva';
select * from linnad
select * from logi
///

<img width="585" height="434" alt="{A9920DA6-8051-4A2F-82AC-56AD1F44F022}" src="https://github.com/user-attachments/assets/62b5d4f0-b788-4fcc-96b6-1ff62214beb9" />

///
--kasutaja sekretaarLiisa, parool 12345
--Õigused - sekretaarLiisa ei saa luua ehk muuta trigeri, ei näi tabeli logi,
--saab ainult näha 

grant select, insert, delete on linnad to sekretaarLiisa;
deny select, delete on logi to sekretaarLiisa;
///


<img width="521" height="593" alt="{03AFCC1F-99A3-4C69-A77A-62E2DC5CC30B}" src="https://github.com/user-attachments/assets/03b7fdd7-4d1d-4790-ae52-e29d65428158" />


<img width="526" height="571" alt="{05FC89F5-9FB0-4BB6-A8B8-B768DE286078}" src="https://github.com/user-attachments/assets/63edca50-084d-4c1e-b6f5-1c32820867da" />


