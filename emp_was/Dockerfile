
FROM ibmcom/websphere-traditional

COPY EmployeeApp.war /tmp/

COPY db2*.jar /tmp/

RUN wsadmin.sh -lang jython -conntype NONE -c "AdminTask.createAuthDataEntry('[-alias db2alias -user db2instl -password db2instl -description ]')"

RUN wsadmin.sh -lang jython -conntype NONE -c "AdminJDBC.createJDBCProvider('DefaultNode01', 'server1', 'DB2JDBCProvider',\
'com.ibm.db2.jcc.DB2ConnectionPoolDataSource',[['classpath', '/tmp/db2jcc4.jar;/tmp/db2jcc_license_cu.jar'],['description','testing'], ['xa','false'],['providerType','DB2 Universal JDBC Driver Provider']])"

RUN wsadmin.sh -lang jython -conntype NONE -c "AdminJDBC.createDataSourceUsingTemplate('DefaultNode01', 'server1', 'DB2JDBCProvider','DB2 Universal JDBC Driver DataSource(templates/system|jdbc-resource-provider-templates.xml#DataSource_DB2_UNI_1)','EmployeeDS',[['authDataAlias','DefaultNode01/db2alias'],['authMechanismPreference','BASIC_PASSWORD'],['description','Employee'], ['jndiName', 'jdbc/empDS'], ['logMissingTransactionContext', 'true'], ['statementCacheSize', '60'],['propertySet', [['resourceProperties',[[['name', 'databaseName'],['type','String'],['value','SAMPLE']],[['name','driverType'],['type','integer'],['value',4]],[['name','serverName'],['type','String'],['value','db2']], [['name', 'portNumber'], ['type', 'integer'],['value', 50000]]]]]]])"

RUN wsadmin.sh -lang jython -conntype NONE -c "AdminApp.install('/tmp/EmployeeApp.war', '[ -appname EmployeeApp -contextroot /Employee -MapResRefToEJB [[ EmployeeApp .* EmployeeApp.war,WEB-INF/web.xml jdbc/empDS javax.sql.DataSource jdbc/empDS "" "" "" ]] -MapWebModToVH [[ EmployeeApp EmployeeApp.war,WEB-INF/web.xml default_host ]]]')"
