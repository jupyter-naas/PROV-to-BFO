# Guide to Mapping a New Ontology to BFO/CCO/RO

This document provides step-by-step instructions for mapping a new ontology to the Basic Formal Ontology (BFO), Common Core Ontologies (CCO), and Relations Ontology (RO) using our established methodology.

## Prerequisites

1. **Required Software**
   - [Protégé](https://protege.stanford.edu/) (version 5.6.3 or higher)
   - [GNU Make](https://www.gnu.org/software/make/) (version 3.81 or higher)
   - Java Runtime Environment (for ROBOT)
   - Git

2. **Required Knowledge**
   - Basic understanding of OWL/RDF ontologies
   - Basic understanding of semantic web technologies
   - Familiarity with your source ontology
   - Basic command line usage

3. **Required Files**
   - Your source ontology file (OWL/RDF format)
   - Latest versions of BFO, CCO, and RO ontologies

## Setup Process

1. **Fork the Template Repository**
   ```bash
   git clone https://github.com/BFO-Mappings/PROV-to-BFO.git your-ontology-to-bfo
   cd your-ontology-to-bfo
   rm -rf .git
   git init
   ```

2. **Update Configuration**
   Edit `src/Makefile`:
   ```makefile
   config.ONTOLOGY_FILE     := your-ontology-edit.ttl
   config.ONTOLOGY_PREFIX   := your-ontology-bfo-directmappings
   config.ONTOLOGY_IRI      := https://your-repo-url/main
   config.ALIGNMENT_FILES   := your-ontology-bfo-directmappings.ttl your-ontology-ro-directmappings.ttl your-ontology-cco-directmappings.ttl
   ```

3. **Setup Directory Structure**
   ```bash
   mkdir -p src/imports/YOUR-ONTOLOGY
   mkdir -p src/sparql
   mkdir -p build/lib
   ```

## Mapping Process

### Phase 1: Preparation

1. **Import Your Ontology**
   - Copy your ontology file to `src/imports/YOUR-ONTOLOGY/`
   - Create an import file listing terms to be mapped

2. **Setup Mapping Files**
   Create three new files:
   ```bash
   touch your-ontology-bfo-directmappings.ttl
   touch your-ontology-ro-directmappings.ttl
   touch your-ontology-cco-directmappings.ttl
   ```

3. **Initialize Mapping Files**
   Add basic metadata to each file:
   ```turtle
   @prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
   @prefix rdfs:   <http://www.w3.org/2000/01/rdf-schema#> .
   @prefix owl:    <http://www.w3.org/2002/07/owl#> .
   @prefix your:   <your-ontology-namespace#> .
   # Add other necessary prefixes
   
   <your-mapping-iri>
     a owl:Ontology ;
     rdfs:label "Your Ontology mappings for BFO/CCO/RO"@en ;
     owl:versionInfo "YYYY-MM-DD"@en ;
     # Add other metadata
   .
   ```

### Phase 2: Creating Mappings

1. **Analyze Source Terms**
   ```bash
   # Generate list of terms to map
   make analyze-terms
   ```

2. **Create Direct Mappings**
   For each class/property, add appropriate mapping axioms:
   ```turtle
   your:SomeClass rdf:type owl:Class .
   []  rdf:type owl:Axiom ;
       owl:annotatedSource   your:SomeClass ;
       owl:annotatedProperty rdfs:subClassOf ; 
       owl:annotatedTarget   bfo:SomeOtherClass ;
       sssom:object_label    "Label" ;
       rdfs:comment "Justification for mapping"@en .
   ```

3. **Create Complex Mappings**
   For terms requiring more complex relationships:
   ```turtle
   # Using SWRL Rules
   [ rdf:type swrl:Imp ;
     swrl:body [...] ;
     swrl:head [...] ;
     rdfs:comment "Rule explanation"@en 
   ] .
   
   # Using Property Chains
   your:property owl:propertyChainAxiom (property1 property2) .
   ```

### Phase 3: Testing

1. **Run Basic Tests**
   ```bash
   make test-edit
   ```

2. **Check for Unmapped Terms**
   ```bash
   make unmapped
   ```

3. **Verify Consistency**
   ```bash
   make reason
   ```

4. **Test with Example Instances**
   - Create example instances in your editor file
   - Run reasoner to verify
   ```bash
   make verify
   ```

### Phase 4: Documentation

1. **Add Mapping Justifications**
   For each mapping, ensure proper documentation:
   - Clear rdfs:comment explaining the mapping
   - SSSOM metadata for mapping provenance
   - References to source documentation

2. **Update README**
   - Document any ontology-specific considerations
   - Add usage examples
   - List contributors

3. **Generate Reports**
   ```bash
   make report
   ```

## Common Issues and Solutions

1. **Inconsistent Ontology**
   - Use `make explain` to identify the source
   - Check for conflicting axioms
   - Verify mapping directions

2. **Missing Terms**
   - Verify import file includes all necessary terms
   - Check namespace declarations
   - Ensure proper MIREOT extraction

3. **Reasoner Timeout**
   - Consider breaking down complex rules
   - Optimize property chains
   - Remove unnecessary axioms

## Best Practices

1. **Mapping Creation**
   - Always provide justification for mappings
   - Use the most specific mapping relation possible
   - Document any assumptions made

2. **Testing**
   - Create test cases for each mapping type
   - Include edge cases in examples
   - Verify bidirectional inference

3. **Documentation**
   - Keep comments current
   - Document any mapping patterns
   - Include references to source specifications

## Resources

- [BFO Documentation](https://basic-formal-ontology.org/)
- [CCO Documentation](https://github.com/CommonCoreOntology/CommonCoreOntologies)
- [RO Documentation](https://oborel.github.io/)
- [ROBOT Documentation](https://robot.obolibrary.org/)

## Support

For questions or issues:
1. Check existing GitHub issues
2. Review the documentation
3. Create a new issue with:
   - Clear description
   - Minimal example
   - Error messages
   - Environment details

## License

This methodology is provided under CC0 1.0 Universal.