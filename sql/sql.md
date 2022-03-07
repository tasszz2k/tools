# SQL QUERIES


## 1. POSTGRESQL
### a. Update by id batch
- simple without try-catch block
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
- with try-catch block

```postgresql
DO
$$
    DECLARE
        page_size INT := 100000;
        min_id    BIGINT; max_id BIGINT;
        row_count INT := 0;
    BEGIN
        SELECT MAX(id), MIN(id) INTO max_id,min_id FROM tmp_nodes;
        RAISE NOTICE 'MIN_ID = % - MAX_ID = %',min_id, max_id;

        FOR i IN min_id..max_id BY page_size
            LOOP
                BEGIN
                    -->> sql update ==========================================================
                    UPDATE tmp_nodes
                    SET node_address = REGEXP_REPLACE(node_address, '^cc ', 'chung cư ')
                    WHERE (id >= i
                        AND id < i + page_size)
                      AND node_address LIKE 'cc %';
                    --<< sql update ==========================================================
                EXCEPTION
                    --- >> Handle exception ==========================================================
                    WHEN OTHERS THEN
                        BEGIN
                            RAISE WARNING ' >> [id from % to %] Returned ERROR',i, i + page_size;
                            FOR j IN i..(i + page_size)
                                LOOP
                                    BEGIN
                                        -->> sql update ==========================================================
                                        UPDATE tmp_nodes
                                        SET node_address = REGEXP_REPLACE(node_address, '^cc ', 'chung cư ')
                                        WHERE (id = j)
                                          AND node_address LIKE 'cc %';
                                        --<< sql update ==========================================================

                                    EXCEPTION
                                        --- >> Handle exception ==========================================================
                                        WHEN OTHERS THEN
                                            BEGIN
                                                RAISE WARNING ' >> id=% Returned ERROR ',j;
                                                UPDATE tmp_nodes
                                                SET verified = -1
                                                WHERE id = j;
                                            END;
                                        --- << Handle exception ==========================================================
                                    END;
                                END LOOP;
                        END;
                    --- << Handle exception ==========================================================
                END;
--                 COMMIT;
                GET DIAGNOSTICS row_count = ROW_COUNT;
                RAISE NOTICE '[id from % to %] Returned % rows',i, i + page_size, row_count;
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
