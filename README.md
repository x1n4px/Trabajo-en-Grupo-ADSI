# Grupo NAMI :computer:
![image](https://github.com/x1n4px/Trabajo-en-Grupo-ADSI/blob/main/aux/images/o112tn80rku21.png?raw=true)

[Entrega 1 - Script Modelo E/R](https://github.com/x1n4px/Trabajo-en-Grupo-ADSI/blob/main/entrega1-modeloER/modeloER.txt)


SYSTEM:
select * from dba_role_privs;
grant create view to ts_nami_nacho;



create role estudiante_rol;
grant create session to estudiante_rol;
grant select on ts_nami_nacho.estudiante to estudiante_rol;
grant select on ts_nami_nacho.asistencia to estudiante_rol;

create user est_1 identified by 123 default tablespace ts_nami quota 500K on ts_nami;
grant estudiante_rol to est_1;



grant create any table to ts_nami_nacho;

create view vista_estudiantes
as select dni, nombre, apellidos
from ts_nami_nacho.estudiantes
where dni = user;



create role vocal_responsable_sede_rol;
grant create session to vocal_responsable_sede_rol;
grant insert, update, delete on ts_nami_nacho.aula to vocal_responsable_sede_rol;
grant insert, update, delete on ts_nami_nacho.sede to vocal_responsable_sede_rol;
grant insert, update, delete on ts_nami_nacho.asistencia to vocal_responsable_sede_rol;
grant insert, update, delete on ts_nami_nacho.examen to vocal_responsable_sede_rol;
grant insert, update, delete on ts_nami_nacho.vocal to vocal_responsable_sede_rol;

create user vocal_responsable_1 identified by 123 default tablespace ts_nami quota 500K on ts_nami;
grant vocal_responsable_sede_rol to vocal_responsable_1;


TS_USER_NAMI:
create view vista_estudiantes as
select dni, nombre, apellidos
from estudiante
where dni = USER;

grant select on vista_estudiantes to estudiante_rol;

create view vista_asignacion_estudiante_aula as
select estudiante_dni, examen_aula_código
from asistencia
where estudiante_dni = USER;

grant select on vista_asignacion_estudiante_aula to estudiante_rol;

create view view_vocal_responsable_sede as
select *
from sede
where vocal_dni = USER;
grant select on view_vocal_responsable_sede to vocal_responsable_sede_rol;

create view view_vocal_responsable_aula as
select *
from aula
where sede_código = USER;
grant select on view_vocal_responsable_sede to vocal_responsable_sede_rol;

create view view_vocal_responsable_examen as
select *
from examen
where vocal_dni = USER;
grant select on view_vocal_responsable_examen to vocal_responsable_sede_rol;


create view view_vocal_responsable_asistencia as
select *
from asistencia;
grant select on view_vocal_responsable_asistencia to vocal_responsable_sede_rol;


create view view_vocal_responsable_vocal as
select *
from vocal;
grant select on view_vocal_responsable_vocal to vocal_responsable_sede_rol;
