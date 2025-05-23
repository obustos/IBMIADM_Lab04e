# IBM i Lab04e - Eventos IBMi

Este laboratorio le guía en la ejecución de un entorno automatizado para detectar eventos del sistema IBM i, capturarlos con un watch (`STRWCH`) y almacenarlos en una tabla logs y comunicar el evento a otras areas de la organización.

---

## 📦 Repositorio Git

Inicia una sesion de Terminal en IBM i y ubiquese en su **homedir**
Clone este repositorio para acceder al material del laboratorio:


```bash
git clone https://github.com/obustos/IBMIADM_Lab04e.git
cd IBMIADM_Lab04e
```

---

## 🔧 Despliegue inicial (Playbook 1)

Este playbook se encarga de :

- Verificar privilegios y elevarlos para poder restarura
- Restaura el objeto `WCHPGMLAB` desde un archivo `.savf`
- Valida de acceso a comandos y objetos relacionados a `watch`

### 🔹 Pasos previos al playbook

> Esta parte la realiza cada estudiante. No es necesario compilar fuentes.

1. Validar que ha realizado exitosamente el lab anterior donde instala ansible en IBM i en un entorno "local" para su perfil de usuario.
2. Ejecuta el playbook 1:

```bash
ansible-playbook playbooks/deploy_watch.yml
```

---

## ⚙️ Ejecución del laboratorio (Playbook 2)

Este playbook activa un watch y genera eventos que deberian ser detectados en un log.
Las actividades sobre archivos 'small' no deben generar evento, los 'big' si derian generalo
- Inicia el watch con `STRWCH`
- Genera eventos (`small` y `big`)
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
