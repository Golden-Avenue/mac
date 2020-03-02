SELECT a.attnum,
a.attname AS field,
t.typname AS type,
a.attlen AS length,
a.atttypmod AS lengthvar,
a.attnotnull AS notnull,
b.description AS comment
FROM pg_class c,
pg_attribute a
LEFT OUTER JOIN pg_description b ON a.attrelid=b.objoid AND a.attnum = b.objsubid,
pg_type t
WHERE c.relname = 'sale_order_line'
and a.attnum > 0
and a.attrelid = c.oid
and a.atttypid = t.oid
ORDER BY a.attnum;

SELECT
  CASE atttypid
         WHEN 21 /*int2*/ THEN 16
         WHEN 23 /*int4*/ THEN 32
         WHEN 20 /*int8*/ THEN 64
         WHEN 1700 /*numeric*/ THEN
              CASE WHEN atttypmod = -1
                   THEN null
                   ELSE ((atttypmod - 4) >> 16) & 65535     -- calculate the precision
                   END
         WHEN 700 /*float4*/ THEN 24 /*FLT_MANT_DIG*/
         WHEN 701 /*float8*/ THEN 53 /*DBL_MANT_DIG*/
         ELSE null
  END   AS numeric_precision,
  CASE
    WHEN atttypid IN (21, 23, 20) THEN 0
    WHEN atttypid IN (1700) THEN           
        CASE
            WHEN atttypmod = -1 THEN null      
            ELSE (atttypmod - 4) & 65535            -- calculate the scale 
        END
       ELSE null
  END AS numeric_scale,
  *
FROM
    pg_attribute where attname='product_uom_qty' ;