# 1.6 Data handling overview

In a BPM-based application, use:

* [Business variables](/busines-data-model) for information that
  * is mostly used by Bonita BPM
  * has meaning after a process instance is archived
  * is used at several places in the process
  * is read by a process to orientate its flow
  * is shared across several process instances
  * is displayed in an application page

* Task-level [process variables](/specify-data-process-definition) for 
  * intermediate computation results (for example a connector result to be used as input of the next connector)
  * information that is private to the process, not used elsewhere in the application

* Pool-level [process variables](/specify-data-process-definition) for information that
  * can be read by the process to orientate its flow
  * can be used at several places in the process
  * is private to the process, not used elsewhere in the application

* External databases, with [connectors](/connectivity-overview-0) and [custom data types](/complex-data-types-0), for information that
  * is used by multiple applications besides Bonita BPM


Key benefits of using business data:

* Control of indexing strategy
* Definition of custom queries to efficiently read the data
* Full control of lifecycle of the data through the set of processes that handle it (add, update and delete)
* Improved performance compared with process variables or external databases
* Easier application maintenance

An application page accesses business data using the [bdm REST API](/bdm-api-0). A page also contains page [variables](/variables), which have no meaning outside the page.


The data that is passed from a form to a process is defined using a [contract](/contracts-and-contexts-0). Define a contract for process instantiation and for each human task. 
These contracts are part of the process definition. 
A form has access to business data and documents using the [context](/contracts-and-contexts-0) and the [REST API](/product-bos-sp/web-rest-api-0). 
A form also contains form [variables](/variables), which have no meaning outside the form.