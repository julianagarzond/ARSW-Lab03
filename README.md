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

4. Make a test program, where an instance of GrammarChecker is created by Spring, and use it



![test](https://user-images.githubusercontent.com/43153078/73946423-e8379200-48c3-11ea-932a-1091f7a35021.PNG)
