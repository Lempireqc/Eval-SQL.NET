---
permalink: split-text
---

## Definition

Improve performance and capability for splitting text with an easy to use split function and LINQ expression

- Split text with multiple delimiters
- Split text using a regular expression
- Include row index

You probably already had to use the fn_split function in the past to split a string with a delimiter. They exists hundreds of variance for this function which are limited to one delimiter.

Using C# code and String.Split or Regex.Split, you are no longer limited and can even use LINQ methods!

### Split using single delimiter

<div class="sqlfiddle">
                <pre class="schema">
                </pre>
                <pre class="sql">
DECLARE @s VARCHAR(MAX) = '1, 2;3, 4|5'

DECLARE @sqlnet SQLNET = SQLNET::New('s.Split(",")').ValueString('s', @s)

-- 1
-- 2;3
-- 4|5
SELECT  *
FROM    dbo.SQLNET_EvalTVF_1(@sqlnet) 
                </pre>
</div>

### Split using multiple delimiters

<div class="sqlfiddle">
                <pre class="schema">
                </pre>
                <pre class="sql">
DECLARE @s VARCHAR(MAX) = '1, 2;3, 4|5'

DECLARE @sqlnet SQLNET = SQLNET::New('s.Split(",", ";", "|")').ValueString('s', @s)

-- 1
--  2
-- 3
--  4
-- 5 
SELECT REPLACE(cast (Value_1 AS VARCHAR(20)),' ', '_') AS Result
FROM    dbo.SQLNET_EvalTVF_1(@sqlnet) 
                </pre>
</div>

### Split and Trim

<div class="sqlfiddle">
                <pre class="schema">
                </pre>
                <pre class="sql">
                DECLARE @s VARCHAR(MAX) = '1, 2;3, 4|5'

DECLARE @sqlnet SQLNET = SQLNET::New('s.Split(",", ";", "|")
                                       .Select(x => x.Trim())')
                                .ValueString('s', @s)

-- 1
-- 2
-- 3
-- 4
-- 5
SELECT  REPLACE(cast (Value_1 AS VARCHAR(20)),' ', '_') AS Result
FROM    dbo.SQLNET_EvalTVF_1(@sqlnet)
                </pre>
</div>

### Split and Trim with Regex

<div class="sqlfiddle">
                <pre class="schema">
                </pre>
                <pre class="sql">
DECLARE @s VARCHAR(MAX) = '1, 2;3, 4|5'

DECLARE @sqlnet SQLNET = SQLNET::New('Regex.Split(s, ",|;|\|")
                                           .Select(x => x.Trim())')
                                .ValueString('s', @s)

-- 1
-- 2
-- 3
-- 4
-- 5
SELECT  REPLACE(cast (Value_1 AS VARCHAR(20)),' ', '_') AS Result
FROM    dbo.SQLNET_EvalTVF_1(@sqlnet)
                </pre>
</div>
