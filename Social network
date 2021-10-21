# ci651-HuaiyuJenna

# 1.1 Install Packages
## For data Wrangling 
library(dplyr)
library(stringr)
library(tidyr)

## For network object
library(network)

## For network graph
library(igraph)

library(visNetwork)
library(networkD3)

# 1.2 Loading data
## Remove missing data
data <- read.csv("data.csv", na= c("","99", "NA", "#N/A"))
## Remove space of each variable name
names(data)<-gsub("\\s","",names(data))

# 3. node list
nodes <- data %>%
  select(id = Author)
                
mentioned_nodes <- data %>% 
  select(id = MentionedAuthors) %>%
  mutate(id = str_replace_all(id, "@", "")) %>%
  separate_rows(id, sep = ", ")

nodes_list <- nodes %>%
  bind_rows(mentioned_nodes) %>%
  distinct(id, .keep_all = T) %>%
  arrange(id)

# 2. edge list
edges_list <- data %>%
  select(V1 = Author, V2 = MentionedAuthors) %>%
  separate_rows(V2, sep = ", ") %>%
  mutate(V2 = str_replace_all(V2, "@", ""))
  
# 3. Directed Network
graph_mention <- graph_from_data_frame(edges_list, directed = TRUE)

## Examine whether the list line up
cbind(names(V(graph_mention)), edges_list$from) %>% 
  head()

# 4. Create node attribute: in-degree
V(graph_mention)$deg.in <- degree(graph_mention, mode = "in")

op_in <- vertex.attributes(graph_mention) %>% 
  as.data.frame()

# 5. Save list of nodes with in-degree 
write_xlsx(op_in, "~/Desktop/DegreeCentralitySD.xlsx")
