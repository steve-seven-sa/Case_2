# Создание тикета для разработчика
Создание тикета разработчику для создания фильтрации списка фильмов и сериалов по эксклюзивности
___
Цель: В библиотеке фильмов и сериалов необходимо добавить фильтр «Эксклюзивно». При его выборе должны отображаться только те фильмы и сериалы, которые доступны эксклюзивно на нашей платформе.
___
Требование: Как пользователь, я хочу видеть подборку эксклюзивных фильмов и сериалов, чтобы не искать их вручную в библиотеке сервиса.
___
Use Case:
   1. В разделе «Библиотека» пользователь выбирает чек-бокс «представлены эксклюзивно».
   2. Система отбирает фильмы и сериалы по тэгу «exclusive».
   3. Система отображает пользователю фильмы и сериалы по заданному фильтру.
___
Описание изменений:
Необходимо доработать метод {{WebServer}}/content/list Web Server API
   1. Добавить параметр запроса «exclusive» типа «boolean».
   2. Добавить параметр ответа «exclusive» типа «boolean».
   3. Добавить обработку параметра запроса «exclusive»:
      + если exclusive=true, то в ответе должен возвращаться только тот контент, у которого поле exclusive=true;
      + если exclusive=false, то возвращается весь контент, который удовлетворяет условиям фильтрации.
___
Контекст: В ответе запроса {{FilmsServer}}/films/list и {{SeriesServer}}/series/list уже есть элемент «exclusive». 
___

**XML-схема GetContentListRequest.xsd**

```
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
<xs:element name="GetContentList">
     <xs:complexType>
<xs:sequence>
<xs:element name="contentType" type="xs:string" />
<xs:element name="genreValue" type="xs:string" />
<xs:element name="exclusive" type="xs:boolean"  />
</xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```
___

**XML-схема GetContentListResponse.xsd**
```
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" 
           xmlns:soap="http://www.w3.org/2003/05/soap-envelope/" 
           soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">
    
    <xs:element name="GetContentListResponse">
        <xs:complexType>
            <xs:sequence>
                
                <xs:element name="contentList">
                    <xs:complexType>
                        <xs:sequence>
                            
                            <xs:element name="content" maxOccurs="unbounded">
                                <xs:complexType>
                                    <xs:sequence>
                                        
                                        <xs:element name="contentType" type="xs:string"/>
                                        <xs:element name="contentId" type="xs:integer"/>
                                        <xs:element name="title" type="xs:string"/>
                                        <xs:element name="description" type="xs:string"/>
                                        <xs:element name="imageUrl" type="xs:string"/>
                                        <xs:element name="previewUrl" type="xs:string"/>
                                        <xs:element name="recordUrl" type="xs:string"/>
                                        
                                        <xs:element name="genre">
                                            <xs:complexType>
                                                <xs:sequence>
                                                    <xs:element name="genreValue" type="xs:string"/>
                                                </xs:sequence>
                                            </xs:complexType>
                                        </xs:element>
                                        
                                        <xs:element name="recommended" type="xs:boolean"/>
                                        <xs:element name="exclusive" type="xs:boolean"/>
                                        
                                        <xs:element name="details">
                                            <xs:complexType>
                                                <xs:sequence>
                                                    
                                                    <xs:element name="yearOfIssue" type="xs:string"/>
                                                    <xs:element name="duration" type="xs:string"/>
                                                    
                                                    <xs:element name="country">
                                                        <xs:complexType>
                                                            <xs:sequence>
                                                                <xs:element name="countryValue" 
                                                                            type="xs:string" 
                                                                            maxOccurs="5"/>
                                                            </xs:sequence>
                                                        </xs:complexType>
                                                    </xs:element>
                                                    
                                                    <xs:element name="ageRate" type="xs:string"/>
                                                    
                                                </xs:sequence>
                                            </xs:complexType>
                                        </xs:element>
                                        
                                        <xs:element name="team">
                                            <xs:complexType>
                                                <xs:sequence>
                                                    
                                                    <xs:element name="cast">
                                                        <xs:complexType>
                                                            <xs:sequence>
                                                                <xs:element name="castValue" 
                                                                            type="xs:string"/>
                                                            </xs:sequence>
                                                        </xs:complexType>
                                                    </xs:element>
                                                    
                                                    <xs:element name="dubbingTeam" type="xs:string"/>
                                                    
                                                </xs:sequence>
                                            </xs:complexType>
                                        </xs:element>
                                        
                                        <xs:element name="rating" type="xs:decimal"/>
                                        
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                            
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
                
            </xs:sequence>
        </xs:complexType>
    </xs:element>
    
</xs:schema>
```
