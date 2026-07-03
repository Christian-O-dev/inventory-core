# Skill: Reglas de seguridad

## Cuándo usar esta skill

Usar en tareas relacionadas con autenticación, autorización, usuarios, roles, permisos, acciones críticas, auditoría, endpoints protegidos o datos sensibles.

## Principios

1. Todo endpoint crítico debe requerir autenticación.
2. Las acciones sensibles deben requerir permisos específicos.
3. El frontend no debe ser la única barrera de seguridad.
4. Las acciones críticas deben registrarse en auditoría.
5. No guardar secretos en código.
6. Usar variables de entorno.
7. Mantener `.env.example` actualizado cuando se agreguen variables.
8. No exponer errores internos al usuario final.
9. No permitir modificaciones destructivas sin control.

## Roles iniciales

- Administrador.
- Vendedor.
- Encargado de almacén.
- Supervisor.

## Acciones críticas

Deben requerir permiso y auditoría:

- Crear o desactivar usuario.
- Cambiar roles.
- Cambiar permisos.
- Crear o modificar producto.
- Cambiar precio.
- Confirmar compra.
- Confirmar venta.
- Cancelar venta.
- Ajustar stock manualmente.
- Desactivar proveedor.

## Riesgos a bloquear

- Vendedor modificando stock manualmente.
- Usuario sin rol accediendo a panel.
- Endpoint protegido solo desde frontend.
- Secretos hardcodeados.
- Token JWT sin expiración.
- Cambios críticos sin audit log.
