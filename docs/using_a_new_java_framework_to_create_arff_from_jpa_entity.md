
A new framework to create ARFF from JPA Entities.

What makes me want to develop this project:

* Not having to worry about parameters of access to the database, since having my application integrated to JPA
* Use Entities (POJO) JPA and its metadata to generate the ARFF when you need to outsource information for use by third parties.
* Generate new Entity (POJO) JPA based on information obtained in the ARFF (planned to release V0.9.0)

Site About Project: [http://socialsla.github.io/Weka-JPA-Persistence](http://socialsla.github.io/Weka-JPA-Persistence)

Source on GitHub: [https://github.com/SocialSLA/Weka-JPA-Persistence](https://github.com/SocialSLA/Weka-JPA-Persistence)


# How code
```
try {
  Weka2JPAHelper l_helper; // inject with CDI or create a class with new Weka2JPAHelper(Logger,EntityManager);

  l_helper.save(new File("Teste.arff"), A_JPA_Entity.class);
}catch(IOException e){}

```
If you want create the data to create a ARFF file from other source, you can use a JPA Entity like the example:

```
try{
        Weka2JPAHelper l_helper = CDIManager.get(Weka2JPAHelper.class);

        Collection<Entity2Weka> l_list = new ArrayList<Entity2Weka>();

        Classification l_positivo = new Classification(1, "Positivo");
        Classification l_negativo = new Classification(-1, "Negativo");
        Classification l_neutro = new Classification(0, "Neutro");

        l_list.add(new Entity2Weka("Post numero 1 para teste do helper", l_positivo));
        l_list.add(new Entity2Weka("Post numero 2 para teste do helper", l_negativo));
        l_list.add(new Entity2Weka("Post numero 3 para teste do helper", l_neutro));
        l_list.add(new Entity2Weka("Post numero 4 para teste do helper", l_negativo));
        l_list.add(new Entity2Weka("Post numero 5 para teste do helper", l_positivo));
        l_list.add(new Entity2Weka("Post numero 6 para teste do helper", l_negativo));
        l_list.add(new Entity2Weka("Post numero 7 para teste do helper", l_positivo));
        l_list.add(new Entity2Weka("Post numero 8 para teste do helper", l_neutro));
        l_list.add(new Entity2Weka("Post numero 9 para teste do helper", l_negativo));

        l_list.add(new Entity2Weka(null, l_positivo));
        l_list.add(new Entity2Weka(null, l_neutro));
        l_list.add(new Entity2Weka(null, l_negativo));

        l_list.add(new Entity2Weka("Post numero 10 para teste do helper", null));
        l_list.add(new Entity2Weka("Post numero 16 para teste do helper", null));
        l_list.add(new Entity2Weka("Post numero 7 para teste do helper", null));
        l_list.add(new Entity2Weka("Post numero 8 para teste do helper", null));
        l_list.add(new Entity2Weka("Post numero 9 para teste do helper", null));

        l_helper.save(new File("Test.arff"), Entity2Weka.class, l_list);

        CDIManager.stop();
    }catch{IOException e){}
	
```

### Entity Example 

The Entity class with name Entity2Weka is a Entity Base used to get attributes and make header file:

```
@Entity
public class Entity2Weka {

    @Id
    private Integer id; // id not is used
    @Column
    private String post; // generate a Sring Attribute
    @ManyToOne
    private Classification classification; // generate a relational attribute

        public Entity2Weka(){}

    public Entity2Weka(String p_string, Classification p_classification) {
        post = p_string;
        classification = p_classification;
    }
}
```

Classification Entity is a slave entity from entity base:

```
@Entity
@NamedQueries({ @NamedQuery(name = Classification.NAMED_QUERY_FIND_ALL, query = "SELECT c FROM Classification c"),
        @NamedQuery(name = Classification.NAMED_QUERY_FIND_BY_NAME, query = "SELECT c FROM Classification c WHERE c.name = ?") })
public class Classification {

    public static final String NAMED_QUERY_FIND_ALL = "Classification.findAll";
    public static final String NAMED_QUERY_FIND_BY_NAME = "Classification.findByName";

    @Id
    private Integer id;
    @Column(length = 50, nullable = false, unique = true)
    private String name;
    @Column(length = 200, nullable = true)
    private String description;

    public Classification() {

    }

    public Classification(int p_i, String p_name) {
        id = p_i;
        name = p_name;
    }
.....
    public toString(){
        return name;
    }
}
```

### Result this ARFF
```
@relation Entity2Weka

@attribute post string
@attribute classification {'Desconhecido (-888)','Negativo (-1)','Neutro (0)','Positivo (1)','Ambiguo (999)'}

@data
'Post numero 1 para teste do helper','Positivo (1)'
'Post numero 2 para teste do helper','Negativo (-1)'
'Post numero 3 para teste do helper','Neutro (0)'
'Post numero 4 para teste do helper','Negativo (-1)'
'Post numero 5 para teste do helper','Positivo (1)'
'Post numero 6 para teste do helper','Negativo (-1)'
'Post numero 7 para teste do helper','Positivo (1)'
'Post numero 8 para teste do helper','Neutro (0)'
'Post numero 9 para teste do helper','Negativo (-1)'
?,'Positivo (1)'
?,'Neutro (0)'
?,'Negativo (-1)'
'Post numero 10 para teste do helper',?
'Post numero 16 para teste do helper',?
'Post numero 7 para teste do helper',?
'Post numero 8 para teste do helper',?
'Post numero 9 para teste do helper',?
```
## Download source

Download this two pakages:

* [WEKA JPA Persistence - V0.0.4](https://github.com/socialsla/weka-jpa-persistence/archive/0.0.4.zip)
* [CDI Utils - for Configuration Injection](https://github.com/socialsla/weka-jpa-persistence/releases/download/0.0.4/cdi-utils-0.0.2.zip)
