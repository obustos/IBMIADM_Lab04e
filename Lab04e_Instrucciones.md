# IBM i Lab04e - Eventos IBMi

Este laboratorio le guía en la ejecución de un entorno automatizado para detectar eventos del sistema IBM i, capturarlos con un watch (`STRWCH`) y almacenarlos en una tabla logs y comunicar el evento a otras areas de la organización.

---

## 📦 Repositorio Git

Inicia una sesion de Terminal en IBM i y ubiquese en su **homedir**
Clone el siguiente repositorio para acceder al material del laboratorio.


```bash
git clone https://github.com/obustos/IBMIADM_Lab04e.git
cd IBMIADM_Lab04e
```

---

## 🔧 Playbook 1: Despliegue inicial 

Este playbook se encarga de :

- Verificar privilegios y elevarlos para poder restarura
- Restaura el objeto `WCHPGMLAB` desde un archivo `.savf`
- Valida de acceso a comandos y objetos relacionados a `watch`

#### 🔹 Pasos previos

1. Validar que ha realizado exitosamente el lab anterior donde instala ansible en IBM i en un entorno "local" para su perfil de usuario.
2. ejeute el comando para validar que tiene version ansible 2.15 on python 3.9+
```bash
ansible --version
```
3. Revise los playbooks clonados con VScode y realice la sustitucion de variables para indicar su User Id
```bash
cd IBMIADM_Lab04e
ls
```

#### 🔹 Despliegue del Watch Service (Programa).

Desde su entorno de terminal, ejecuta el playbook 1:

```bash
ansible-playbook playbooks/deploy_watch.yml
```

---


### ⚙️ Playbook 2: Arranque del Watch (Observación)

Este playbook activa un watch y consulta los monitores activos.

```bash
ansible-playbook playbooks/run_watch.yml
```

---

### ⚙️ Playbook 3: Generación de Eventos

Este playbook genera eventos que deberian ser detectados en un log.
Las actividades sobre archivos 'small' no deben generar evento, los 'big' si derian generalo.

Cree un miembro fuente llamado GEN_EVENT en QSCRIPTS de su schema de trabajo
e ingrese la siguiente instrucción, tambien lo puede copiar de LABADM.
```sql
Call labadm.genera_evento('big', 'IBMIADMxx', '03-desde Ansible PlayBook - CL'); 
```
Ejecute el playbook.

```bash
ansible-playbook playbooks/gen_events.yml
```

Genere un par de eventos adicionales desde cualquiera de las herramientas de automatizacion conocidas como:
- db2util
- XMLService
- RSEAPI

*Nota:* Deberia usar el tag xml para lanzar una sentencia < sql > y/o consumir el API SQL de RSEAPI (no olvide la autenticacion previa).

Oberva este playbook o el miembro fuente para copiar la sentencia SQL que genera los eventos. 

Cambie el 3er parametro indicando el origen del evento ej.: 
- 04-desde PASE-db2util 
- 05-desde Xmlservice 
- 06-desde RSEAPI

Consulte la tabla de logs para validar que ha generado al menos 5 eventos
```sql
Select *
   From labadm.watch_events
   Where Regexp_Like (
      msg_repld,
      'IBMIADMXX',
      'i'
   );
```


---

### ⚙️ Playbook 4: Finalizacion del Watch

Este playbook finaliza el monitoreo o watch activo.

```bash
ansible-playbook playbooks/end_watch.yml
```

En este momento puede generar nuevos eventos pero estos NO serán capturados por el Watch.

---

### ⚙️ Playbook 5: Cierre 

Consulte los archivos big* y small* generados en su schema de trabajo, para ello puede consultar el catálogo SYSTABLES de su schema de trabajo.
```sql
Select * From catalogo where regexp_like(table_name, 'small|big', 'i');
```

Ejecute el playbook de limpieza.

```bash
ansible-playbook playbooks/cleanup_watch.yml
```
---
FIN

---