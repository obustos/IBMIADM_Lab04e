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
cd IBMIADM_Lab04e/playbooks
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
ansible-playbook playbooks/start_watch.yml
```

---

### ⚙️ Playbook 3: Generación de Eventos

Este playbook genera eventos que deberian ser detectados en un log.
Las actividades sobre archivos 'small' no deben generar evento, los 'big' si derian generalo.

Cree un miembro fuente llamado GEN_EVENT en QSCRIPTS de su schema de trabajo e ingrese la siguiente instrucción.
```sql
Call labadm.genera_evento('big', 'IBMIADMxx', '03-desde Ansible PlayBook - CL'); 
```
Ejecute el playbook.

```bash
ansible-playbook playbooks/gen_events.yml
```

Genere un par de eventos adicionales desde cualquiera de las herramientas de automatizacion de su preferencia como:
- db2util
- XMLService
- RSEAPI

*Nota:* Si utiliza xmlservice, deberá usar el tag xml para lanzar una sentencia < sql >. Para RSEAPI debe consumir el API SQL (no olvide la autenticacion previa).

Oberva el playbook gen_events.yml o el miembro fuente GEN_EVENT para copiar la sentencia SQL que genera los eventos. 

Cambie el tercer parametro indicando el origen del nuevo evento Ej.: 
- 04-desde PASE-db2util 
- 05-desde Xmlservice 
- 06-desde RSEAPI

Consulte la tabla de logs para validar que ha generado al menos 4 o 5 eventos.
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

Cada evento lanzado produce dos tareas:
1. Generar un archivo con prefijo SMALL o BIG y una extension pseudoaleatoria (relacionado con el procesamiento de un paquete transaccional). El interes del watch es notificar de la ejecucion de esas transacciones pero solo de paquetes BIG.
2. Enviar un mensaje en QSYSOPR que, de manera asincrona, será captiurado por el watch para enviar la notificacion o alerta, en este caso colectar un log para telemetria u observabilidad del proceso.

Consulte los archivos big* y small* generados en sus eventos, para ello puede consultar el catálogo SYSTABLES de su schema de trabajo.
```sql
Select * From catalogo where regexp_like(table_name, 'small|big', 'i');
```

Ejecute el playbook de limpieza para retirar el watch del sistema y que ya no reporte eventos.

```bash
ansible-playbook playbooks/cleanup_watch.yml
```
---
FIN

---