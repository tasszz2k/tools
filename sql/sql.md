# SQL QUERIES


## 1. POSTGRESQL
### a. Update by id batch
```
DO
$$
    DECLARE
        page      INT := 10000;
        min_id    BIGINT; max_id BIGINT;
        row_count INT := 0;
    BEGIN
        SELECT MAX(id), MIN(id) INTO max_id,min_id FROM tmp_nodes;
        FOR j IN min_id..max_id BY page
            LOOP
                UPDATE tmp_nodes
                SET node_address = REGEXP_REPLACE(node_address, '^0 ', ' ')
                WHERE (id >= j
                    AND id < j + page)
                  AND node_address LIKE '0 %';
--                 COMMIT;
                GET DIAGNOSTICS row_count = ROW_COUNT;
                RAISE NOTICE '[id from % to %] Returned % rows',j, j + page, row_count;
            END LOOP;
    END;
$$;
```


## 2. Title
+ Note
+ **link: [link](url)**

### Function: 
+ Draw Diagram


## 3. Title
+ Note
+ **link: [link](url)**

### Function: 
+ Draw Diagram

## 4. Title
+ Note
+ **link: [link](url)**

### Function: 

# II. OTHER TOOLS

## 1. Title
+ Note
+ **link: [link](url)**

### Function: 
