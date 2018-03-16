# cypher (neo4j)
Cypher is a declarative graph query language that allows for expressive and efficient querying and updating of the graph store.

# 1. clauses
## 1.1. CREATE (and DELETE)
Create (and delete) nodes and relationships.

### examples
#### create node with properties
```
CREATE (harley01:Motorbike {name:"Harley Davidson", model:"XLCR 1000 Café Racer", year:"1978"})
RETURN harley01
```

#### create nodes with properties and a relationship between them with properties
```
CREATE (switch001:Switch {name:"switch-001"}),
  (switch002:Switch {name:"switch-002"}),
  (switch001)-[:TRUNK {trunk:1, port:23, protocol:'LACP'}]->(switch002)
RETURN switch001, switch002
```

#### create relationship with properties between existing nodes
```
MATCH (n:Switch {name:'switch-002'}), (m:Switch {name:'switch-001'})
CREATE (n)-[:TRUNK {trunk:19}]->(m)
```

#### delete all nodes
```
MATCH (n) DETACH DELETE n
```

#### delete all relationships with certain type
```
MATCH ()-[x:Contains]-() 
DELETE x
```

#### delete specific relationship
```
MATCH (:Switch {name: "switch-001"})-[x:Trunk]-(:Switch {name: "switch-002"}) 
DELETE x
```

#### delete certain nodes by type and name
```
MATCH (x:Rack {name:'rack-001'})
DETACH
DELETE (x)
```

#### delete nodes with ID higher than
```
MATCH (s)
WHERE ID(s) > 120
DETACH DELETE s
```

## 1.2. SET (and REMOVE)
Set values to properties and add labels on nodes using SET and use REMOVE to remove them.

## 1.3. MERGE
Match existing or create new nodes and patterns. This is especially useful together with unique constraints.

### examples
#### if node doesn't exist with certain name and properties, create it
```
MERGE (rack001:Rack { name: 'rack-001', space:48 })
RETURN rack001
```

#### if specific relationship doesn't exist, create it (with properties)
```
MATCH (n:Rack {name:'rack-001'}), (m:Switch {name:'switch-001'})
MERGE (n)-[:Contains {location:'u19'}]->(m)
RETURN n, m
```

#### merge multiple relationships
```
MATCH (n:Rack {name:'rack-001'}), (m:Switch {name:'switch-001'}), (o:Switch {name:"switch-002"})
MERGE (n)-[:Contains {location:"u2"}]->(m)
MERGE (n)-[:Contains {location:"u3"}]->(o)
RETURN n, m, o
```

## 1.4. MATCH
The graph pattern to match. This is the most common way to get data from the graph.

### examples
#### match (show) all nodes and relationships

```
MATCH (n)
RETURN n
```

#### match node by ID
```
MATCH (s)
WHERE ID(s) = 5413
RETURN s
```

#### match node by property
```
MATCH (s)
WHERE s.name = "John"
RETURN s
```

### match node with certain type, return certain property of matched nodes
```
MATCH (n:Switch)
RETURN n.name
```

## 1.5. WHERE
Not a clause in its own right, but rather part of MATCH, OPTIONAL MATCH and WITH. Adds constraints to a pattern, or filters the intermediate result passing through WITH.

## 1.6. RETURN
What to return.

### inserting comments
#### line comment
```
//This is a line comment
```

# 2. sources
Cypher Manual: https://neo4j.com/docs/developer-manual/3.3/cypher/

