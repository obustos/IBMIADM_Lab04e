# 🧪 Laboratorio 04E – Automatización de eventos y monitoreo en IBM i con Ansible

Este laboratorio tiene como objetivo mostrar a los estudiantes cómo aprovechar la automatización y el monitoreo de eventos en **IBM i 7.5**, utilizando herramientas como:

- ✅ Ansible (automatización basada en YAML)
- ✅ Programas `watch` (`STRWCH` / `ENDWCH`)
- ✅ Captura de eventos vía mensajes del sistema (`CPF*`)
- ✅ Inserción de eventos en una tabla con control de acceso (RCAC)
- ✅ Generación de reportes automatizados

---

## 🎯 ¿Por qué es importante este laboratorio?

Porque te permitirá experimentar con un flujo completo de observabilidad sobre IBM i que incluye:

- Activación de monitoreo de eventos
- Registro automático de actividad según condiciones preestablecidas
- Desacoplamiento entre quien genera el evento y quien lo observa
- Uso de Ansible como plataforma de despliegue repetible y estructurada
- Control de visibilidad mediante RCAC

Además, es una excelente oportunidad para conocer cómo los sistemas centrales pueden beneficiarse de enfoques modernos sin dejar atrás su robustez.

---

## 🚀 ¿Qué vas a hacer?

Durante este laboratorio:

1. Restaurarás un programa de tipo `watch` desde un `.savf` automatizado
2. Lanzarás el monitoreo de eventos con un `playbook`
3. Generarás eventos de prueba
4. Detendrás el `watch` y consultarás los eventos capturados
5. Revisarás los mensajes del sistema asociados
6. Opcionalmente limpiarás los artefactos de la prueba

---

## 📘 ¿Cómo empiezo?

Revisa el archivo [`lab04e_instruciones.md`](./lab04e_instruciones.md) para seguir los pasos recomendados.

> Este laboratorio está diseñado para ser ejecutado **con mínima intervención manual**, aprovechando al máximo las capacidades de automatización que ofrece Ansible en IBM i.

---

¡Éxitos y que disfrutes automatizando el poder de IBM i! 💡

