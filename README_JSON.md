`psql -h 127.0.0.1 -U postgres development < versioning_function_json.sql`

Replace `original_table` with the table name in the code below.

```SQL
ALTER TABLE original_table ADD COLUMN sys_period tstzrange NOT NULL DEFAULT tstzrange(current_timestamp, null);
```

```SQL
CREATE TABLE original_table_history (
  id INTEGER,
  sys_period tstzrange NOT NULL,
  json_data JSONB
);
```

```SQL
CREATE TRIGGER versioning_trigger_original_table
BEFORE INSERT OR UPDATE OR DELETE ON original_table
FOR EACH ROW EXECUTE PROCEDURE versioning('sys_period', 'original_table_history', true);
```