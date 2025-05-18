# LABORATORIO: IBM i Watch + RCAC + Ansible

Este laboratorio te guía en la ejecución de un entorno automatizado para detectar eventos del sistema IBM i, capturarlos con un programa watch (`STRWCH`) y almacenarlos en una tabla protegida con RCAC.

---

## 📦 Repositorio Git

Clona este repositorio para acceder al material del laboratorio:

```bash
git clone https://github.com/tu-org/ibmi-watch-lab.git
cd ibmi-watch-lab
```

---

## 🔧 Despliegue inicial (Playbook 1)

Este playbook realiza:

- Restauración del objeto `WCHPGMLAB` desde un archivo `.savf`
- Verificación de privilegios requeridos
- Validación de acceso a comandos relacionados a `watch`

### 🔹 Pasos previos al playbook

> Esta parte la realiza cada estudiante. No es necesario compilar fuentes.

1. Verifica que el archivo `wchpgmlab.savf.zip` esté disponible en `savf/`
2. Descomprime el archivo:
   ```bash
   unzip savf/wchpgmlab.savf.zip -d /tmp
   ```
3. Copia el archivo `.savf` a IBM i:
   ```bash
   CPYFRMSTMF FROMSTMF('/tmp/wchpgmlab.savf') TOMBR('/qsys.lib/misavfs.lib/wchpgmlab.file')
   ```
4. Ejecuta el playbook:

```bash
ansible-playbook playbooks/deploy_watch.yml
```

---

## ⚙️ Ejecución del laboratorio (Playbook 2)

Este playbook:

- Inicia el watch con `STRWCH`
- Genera 5 eventos (`small`, `big`)
- Finaliza el watch
- Consulta la tabla `event_log` filtrada por usuario

```bash
ansible-playbook playbooks/run_watch.yml
```

También puedes consultar manualmente:

```sql
SELECT * FROM LABADM.EVENT_LOG ORDER BY TIMESTAMP DESC;
```

---

## 📌 Nota final
Este laboratorio asume que el instructor ya activó RCAC sobre `LABADM.EVENT_LOG`, por lo tanto, solo verás tus propios eventos al consultar.
