DOM:

Què és?
It is a programming interface for HTML and XML documents.
It is a programming interface that allows us to create, change or delete elements of the document. We can also add events to these elements to liven up our page.


(Es una interfaz de programación para los documentos HTML y XML.)
(És una interfície de programació que ens permet crear, canviar o eliminar elements del document. També podem afegir esdeveniments a aquests elements per dinamitzar la nostra pàgina.)

Com funciona?
The DOM is a programming API for documents. It has a great similarity with the structure of the document it models. For example, consider this table, take an HTML document:

![image](https://user-images.githubusercontent.com/91152783/200332532-240fe9a9-0f3d-491d-93fd-dcc1b65dc3b2.png)


The DOM represents this table like this:



![image](https://user-images.githubusercontent.com/91152783/200331585-f2fcc894-3f22-4bd4-9858-cc08d80906c7.png)





The DOM is a logical model that can be implemented in any way that is convenient. In this specification, we use the term structure model to describe the tree-like representation of a document


Classes necessàries?
DocumentBuilderFactory
DocumentBuilder
Document
NodeList

Excepcions que s'han de controlar?
Exception
Codi d'exemple de lectura des de fitxer?
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import java.io.File;

public class Main {
    public static void main(String[] args) {

        File file = new File("cd_catalog.xml");

        // LECTURA DE FITXER
        try {
            DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
            Document doc = dBuilder.parse(file);

            // NORMALIZE XML DOCUMENT
            doc.getDocumentElement().normalize();

            // SAVE THE NODES AND SHOW CD's COUNT
            NodeList nList = doc.getElementsByTagName("CD");
            System.out.println("Número de CD: " + nList.getLength());

            for (int i = 0; i < nList.getLength(); i++) {
                Node nNode = nList.item(i);

                if (nNode.getNodeType() == Node.ELEMENT_NODE) {
                    Element eElement = (Element) nNode;

                    System.out.println("TITLE: "
                            + eElement.getElementsByTagName("TITLE").item(0).getTextContent());
                    System.out.println("ARTIST: "
                            + eElement.getElementsByTagName("ARTIST").item(0).getTextContent());
                    System.out.println("COUNTRY: "
                            + eElement.getElementsByTagName("COUNTRY").item(0).getTextContent());
                    System.out.println("COMPANY: " + eElement.getElementsByTagName("COMPANY").item(0).getTextContent());
                    System.out.println("PRICE: "+eElement.getElementsByTagName("PRICE").item(0).getTextContent());
                    System.out.println("YEAR: "+eElement.getElementsByTagName("YEAR").item(0).getTextContent());
                    System.out.println();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}


![image](https://user-images.githubusercontent.com/91152783/200331847-2ada1415-2ee4-4dc1-9a58-3e3a78c7c5c2.png)


Codi d'exemple d'escriptura a fitxer?
import org.w3c.dom.Attr;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;

public class DOM {
    public static void main(String[] args) {

        try {
            DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
            DocumentBuilder db = dbf.newDocumentBuilder();
            Document doc = db.newDocument();

            
            Element rootElement = doc.createElement("catalog");
            doc.appendChild(rootElement);

           
            Element cdElement = doc.createElement("cd");
            rootElement.appendChild(cdElement);

           
            Attr cdIdAttr = doc.createAttribute("id");
            cdIdAttr.setValue("1");
            cdElement.setAttributeNode(cdIdAttr);

          
            Element titleElement = doc.createElement("title");
            titleElement.appendChild(doc.createTextNode("CD"));
            cdElement.appendChild(titleElement);

            Element artistElement = doc.createElement("artist");
            artistElement.appendChild(doc.createTextNode("ARTIST"));
            cdElement.appendChild(artistElement);

            Element countryElement = doc.createElement("country");
            countryElement.appendChild(doc.createTextNode("COUNTRY"));
            cdElement.appendChild(countryElement);

            Element companyElement = doc.createElement("company");
            companyElement.appendChild(doc.createTextNode("COMPANY"));
            cdElement.appendChild(companyElement);

            Element priceElement = doc.createElement("price");
            priceElement.appendChild(doc.createTextNode("PRICE"));
            cdElement.appendChild(priceElement);

            Element yearElement = doc.createElement("year");
            yearElement.appendChild(doc.createTextNode("YEAR"));
            cdElement.appendChild(yearElement);

            //XML DOCUMENT CREATION
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            DOMSource source = new DOMSource(doc);
            StreamResult result = new StreamResult(new File("catalog_copy.xml"));

            transformer.transform(source,result);

        }catch (Exception e){
            e.printStackTrace();
        }
    }
}

![image](https://user-images.githubusercontent.com/91152783/200331935-a964b5c5-95b5-4993-8b68-7ee70dffdd84.png)











SAX:
Expliqueu els aspectes fonamentals de SAX:
  Què és.
      SAX is an XML parsing standard.Unlike DOM, it processes information by events
  Com funciona.
      SAX parsers are based on an event model: as the parser traverses the document it reports the occurrence of events, such as the start of an XML element or the end of the document, to an event handler.
      
  Classes necessàries.
  Excepcions que s'han de controlar.
  Codi d'exemple de lectura des de fitxer.
  Codi d'exemple d'escriptura a fitxer.





XPath	

Què és Xpath?
XPath is a language that allows you to query an XML document in a simple way.

   • Què puc fer amb Xpath?
Find information from an XML document with filters using formulas
    • Posa un exemple de com consultar els CDs anteriors a 1990.

    import java.io.File;

    import javax.xml.parsers.DocumentBuilder;
    import javax.xml.parsers.DocumentBuilderFactory;
    import javax.xml.xpath.XPath;
    import javax.xml.xpath.XPathConstants;
    import javax.xml.xpath.XPathFactory;

    import org.w3c.dom.Document;
    import org.w3c.dom.NodeList;

    public class PruebaXPath {
        public static void main(String[] args) throws Exception {
            // La expresion xpath de busqueda
            String xPathExpression = "//CATALOG/CD [YEAR < 1990]";

            // Carga del documento xml
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document documento = builder.parse(new File("cd_catalog.xml"));

            // Preparación de xpath
            XPath xpath = XPathFactory.newInstance().newXPath();

            // Consultas
            NodeList nodos = (NodeList) xpath.evaluate(xPathExpression, documento, XPathConstants.NODESET);

            for (int i=0;i<nodos.getLength();i++){
                System.out.println(nodos.item(i).getNodeName()+" : " +
                        nodos.item(i).getTextContent());
            }
        }
    }


![image](https://user-images.githubusercontent.com/91152783/200332011-f10852aa-58ef-4979-8fb3-da60ac67764a.png)







XQuery


Què és Xquery?
Is a query language designed for XML data collections. It is semantically similar to SQL, although it includes some programming capabilities.

Què puc fer amb Xquery?
    XQuery provides the means to extract and manipulate information from XML documents, or from any data source that can be represented by means of XML, such as, for example, relational databases or office documents.
      
      
 Posa un exemple de com consultar els CDs més barats de 10$.
