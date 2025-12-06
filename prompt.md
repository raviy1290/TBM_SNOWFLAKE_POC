Initial Design Request: 
"Help design a software system for business and financial views of enterprise spend, using vibe coding in TBM style. Start step-by-step or outline your approach first."

Scope to Phases A/B: 
"Focus on Phases A and B only. What are typical data sources covering 90% of enterprise costs? Use top three: cloud/infrastructure, labor, and SaaS."

Post-Ingestion Processing: 
"After raw cost data ingestion, describe cost event normalization—what it does before mapping to cost towers."

Cost Tower Mapping: 
"Explain typical cost tower mapping process. Use https://www.apptio.com/platform/atum/ as additional context."

Layer Advantages: 
"What advantages do additional layers like cost pools, IT towers, and applications provide? Why are cost pools important alongside towers?"

Development Prompt: 
"Freeze the design. Generate a development prompt for AWS Kiro vibe coding to implement it, but build the entire solution on Snowflake."

High-Level Design: 
"Outline a typical Snowflake-based design for this system, including key queries, transformations, and materialized views. Exclude lineage; keep minimal MVP scope."

Compact MVP Options: 
"Suggest options for an ultra-compact MVP."

Snowflake Pipeline MVP: 
"Select Option 3: Build as a Snowflake-native pipeline. Start small and explain your steps."

Raw Table Handling: 
"For the raw table, does CUR file provide daily cost summaries? If not, adjust. SaaS and labor remain unchanged. Split SaaS like Databricks/Snowflake (90% engineering, 10% business)."
