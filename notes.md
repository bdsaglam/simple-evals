## Run evals

```sh
python simple_evals.py run drop --model deepseek-r1-llama-8b
```

```sh
python simple_evals.py run drop --model deepseek-r1-llama-8b -n 10 --out ./tmp/subset-10
```


## Logfire dashboard

```sql
WITH traces AS (
    SELECT trace_id, message, duration, 
           json_get_int(attributes, 'response_data', 'usage', 'prompt_tokens') AS prompt_tokens, 
           json_get_int(attributes, 'response_data', 'usage', 'completion_tokens') AS completion_tokens, 
           json_get_int(attributes, 'response_data', 'usage', 'total_tokens') AS total_tokens
    FROM records
    WHERE is_exception IS false
)
SELECT     
    COUNT(*) AS record_count,
    MIN(prompt_tokens) AS min_prompt_tokens,
    AVG(prompt_tokens) AS avg_prompt_tokens,
    MAX(prompt_tokens) AS max_prompt_tokens,
    MIN(completion_tokens) AS min_completion_tokens,
    AVG(completion_tokens) AS avg_completion_tokens,
    MAX(completion_tokens) AS max_completion_tokens,
    MIN(total_tokens) AS min_total_tokens,
    AVG(total_tokens) AS avg_total_tokens,
    MAX(total_tokens) AS max_total_tokens
FROM traces
WHERE completion_tokens IS NOT NULL AND message = 'Chat Completion with ''deepseek-r1-llama-70b'''

```