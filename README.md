# ARSW-Lab03

## Members
  - Juliana Garzón Duque
  - Eduardo Ocampo Arellano 
  
## Compile
  - mvn pacakage
  
## Run
  - mvn compile


## Part I - Basic workshop 
To illustrate the use of the Spring framework, and the development environment for its use through Maven (and NetBeans), the       configuration of a text analysis application will be made, which makes use of a grammar verifier that requires a spelling checker. The grammar checker will be injected, at the time of execution, with the spelling checker required (for now, there are two available: English and Spanish).
1. Open the project sources in NetBeans.
2. Review the Spring configuration file already included in the project (src / main / resources). It indicates that Spring will       automatically search for the 'Beans' available in the indicated package.
3. Making use of the Spring configuration based on annotations mark with the annotations @Autowired and @Service the dependencies that must be injected, and the 'beans' candidates to be injected -respectively-:
GrammarChecker will be a bean, which depends on something like 'SpellChecker'.
EnglishSpellChecker and SpanishSpellChecker are the two possible candidates to be injected. One must be selected, or another, but NOT both (there would be dependency resolution conflict). For now, have EnglishSpellChecker used. 

![Capture1](https://user-images.githubusercontent.com/43153078/73947218-1ec1dc80-48c5-11ea-89c3-74be61cac403.PNG)
![Capture2](https://user-images.githubusercontent.com/43153078/73947358-4ca72100-48c5-11ea-87e4-d29eba9c2090.PNG)

4. Make a test program, where an instance of GrammarChecker is created by Spring, and use it



![test](https://user-images.githubusercontent.com/43153078/73946423-e8379200-48c3-11ea-932a-1091f7a35021.PNG)

### Blueprint Management 1
## Part 1

![assets_-LWJN2LirJZqzEmpZ3Gn_-LmoBwcRZgykpBDUEYdn_-LmoD5BKRb7fFj_aqsS1_BluePrint (1)](https://user-images.githubusercontent.com/43153078/74153438-9f8f1a00-4bde-11ea-95ef-1e1f9a7b0c56.png) 

Configure the application to work under a dependency injection scheme, as shown in the previous diagram.
The above requires:
1. Add the dependencies of Spring. Add the Spring settings. Configure the application - by means of annotations - so that the persistence scheme is injected when the BlueprintServices bean is created. Complete the getBluePrint() and getBlueprintsByAuthor() operations. Implement everything required from the lower layers (for now, the available persistence scheme InMemoryBlueprintPersistence) by adding the corresponding tests in InMemoryPersistenceTest.

InMemoryBlueprintPersistence class 
  ``` java
  @Override
    public Blueprint getBlueprint(String author, String bprintname) throws BlueprintNotFoundException {
        return blueprints.get(new Tuple<>(author, bprintname));
    }
  
```
BlueprintsPersistence class 
 ``` java
  public Blueprint getBlueprint(String author,String name) throws BlueprintNotFoundException{
        return bpp.getBlueprint(author, name);
    }
    
  ```
  
  InMemoryBlueprintPersistence class 
  ``` java
   public Set<Blueprint> getBlueprintByAuthor(String auth) {
        Set<Blueprint> authorBlueprints = new HashSet<>();
        for (Tuple<String, String> key : blueprints.keySet()) {
            if (key.getElem1().equals(auth)) {
                authorBlueprints.add(blueprints.get(key));
            }
        }
        return authorBlueprints;
    }
    
  ```
  BlueprintsPersistence class
   ``` java
    public Set<Blueprint> getBlueprintsByAuthor(String author) throws BlueprintNotFoundException {
        return bpp.getBlueprintByAuthor(author);
    }
  
  ```
  ``` java
   @Test
    public void getBlueprintTest(){
        InMemoryBlueprintPersistence ibpp = new InMemoryBlueprintPersistence();
        Point[] pts = new Point[]{new Point(0, 0),new Point(10, 10)};
        Blueprint bp = new Blueprint("john", "theRipper",pts);
        try {
            ibpp.saveBlueprint(bp);
        } catch (BlueprintPersistenceException e) {
            fail("InMemoryBluePrintsPersistence save error.");
        }
        Blueprint resultBp=null;
        try {
            resultBp = ibpp.getBlueprint("john","theRipper");
        } catch (BlueprintNotFoundException e) {
            fail("InMemoryBluePrintsPersistence get error.");
        }
        assertEquals(resultBp,bp);
    }

    @Test
    public void getBlueprintByAuthorTest(){
        InMemoryBlueprintPersistence ibpp = new InMemoryBlueprintPersistence();
        Point[] pts1 = new Point[]{new Point(0, 0),new Point(10, 10)};
        Blueprint bp1 = new Blueprint("john", "thepaint",pts1);
        Point[] pts2 = new Point[]{new Point(0, 45),new Point(45, 10)};
        Blueprint bp2 = new Blueprint("john", "theRipper",pts2);
        Point[] pts3 = new Point[]{new Point(23, 43),new Point(56, 10)};
        Blueprint bp3 = new Blueprint("juan", "saltaMuros",pts3);
        Set<Blueprint> authorBlueprints = new HashSet<>();
        authorBlueprints.add(bp1);
        authorBlueprints.add(bp2);

        try {
            ibpp.saveBlueprint(bp1);
            ibpp.saveBlueprint(bp2);
            ibpp.saveBlueprint(bp3);
        } catch (BlueprintPersistenceException ex) {
            fail("InMemoryBluePrintsPersistence save error.");
        }
        assertEquals(ibpp.getBlueprintByAuthor("john"),authorBlueprints);
    }
}
```
  
    
    
 


2. Make a program in which you create (through Spring) an instance of BlueprintServices, and rectify its functionality: register plans, consult plans, register specific plans, etc.
3. You want the plan query operations to perform a filtering process, before returning the planes consulted. These filters are looking to reduce the size of the plans, removing redundant data or simply sub-sampling, before returning them. Adjust the application (adding the abstractions and implementations you consider) so that the BlueprintServices class is injected with one of two possible 'filters' (or possible future filters). The use of more than one at a time is not contemplated:

  3.1.  (A) Redundancy filtering: deletes consecutive points from the plane that are repeated.
  
  3.2.  (B) Subsampling filtering: suppresses 1 out of every 2 points in the plane, interspersed.
  
4. Add the corresponding tests to each of these filters, and test its operation in the test program, verifying that only by changing the position of the annotations - without changing anything else - the program returns the filtered planes in the way (A) or in the way (B).






