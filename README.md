Pl/SQL block to convert charset:

```
set serveroutput on

BEGIN
  FOR linha in (SELECT 'ALTER TABLE "'||a.OWNER||'"'||'.'||'"'||a.TABLE_NAME||'"'||' MODIFY ('||'"'||a.COLUMN_NAME||'"'||' '||a.DATA_TYPE||'('||a.DATA_LENGTH||' CHAR))' cmd
   FROM DBA_TAB_COLUMNS a, DBA_TABLES t
   WHERE A.OWNER = T.OWNER
   AND A.TABLE_NAME = T.TABLE_NAME
   AND CHAR_USED = 'B'
   AND a.OWNER NOT IN ('SYS','SYSTEM','OUTLN','MDSYS','ORDSYS','EXFSYS','DBSNMP','WMSYS','APPQOSSYS','OWBSYS_AUDIT','ORDDATA','CTXSYS','ANONYMOUS','XDB','ORDPLUGINS','OWBSYS','SI_INFORMTN_SCHEMA','OLAPSYS','ORACLE_OCM','XS$NULL','MDDATADIP','SPATIAL_CSW_ADMIN_USR','SPATIAL_WFS_ADMIN_USR', 'SSB','AUDSYS','GSMADMIN_INTERNAL','DVSYS','ODI_OPSS','APEX_190100','ODI_IAU','GGADMIN', 'ODI_WLS_RUNTIME', 'ODI_WLS', 'ORDS_METADATA','ODI_STB', 'FMWREGISTRY','C##CLOUD$SERVICE', 'FLOWS_FILES', 'LBACSYS', 'SH', 'ADMIN', 'DBSFWUSER','APEX_190200')
   AND t.TABLE_NAME NOT IN (SELECT r.OBJECT_NAME
                              FROM DBA_RECYCLEBIN r
                             WHERE a.owner = r.owner)) LOOP

     dbms_output.put_line (linha.cmd);
     BEGIN
       execute immediate linha.cmd;    
     EXCEPTION
	When others then
          dbms_output.put_line('erro: '|| sqlerrM ||' - '|| linha.cmd);
     end;
  END LOOP;
END;
/

```
