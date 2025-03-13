# Tablespaces

## Total, Free, Used space in tablespaces

````sql
SET LINESIZE 200
COL TABLESPACE_NAME FORMAT A20
COL total_space_mb FORMAT 999,999,999.99
COL used_space_mb FORMAT 999,999,999.99
COL free_space_mb FORMAT 999,999,999.99
COL used_percentage FORMAT 999.99
COL ENCRYPTED FORMAT A10

SELECT 
    df.tablespace_name,
    df.total_space_mb,
    NVL(fs.free_space_mb, 0) AS free_space_mb,
    (df.total_space_mb - NVL(fs.free_space_mb, 0)) AS used_space_mb,
    ROUND((1 - NVL(fs.free_space_mb, 0) / df.total_space_mb) * 100, 2) AS used_percentage,
    NVL(te.encrypted, 'NO') AS encrypted
FROM 
    (SELECT tablespace_name, SUM(bytes) / 1024 / 1024 AS total_space_mb 
     FROM dba_data_files 
     GROUP BY tablespace_name) df
LEFT JOIN 
    (SELECT tablespace_name, SUM(bytes) / 1024 / 1024 AS free_space_mb 
     FROM dba_free_space 
     GROUP BY tablespace_name) fs
ON df.tablespace_name = fs.tablespace_name
LEFT JOIN 
    (SELECT tablespace_name, encrypted 
     FROM dba_tablespaces) te
ON df.tablespace_name = te.tablespace_name
ORDER BY df.tablespace_name;
````
