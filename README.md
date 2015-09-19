# NHibernate.Cache.Couchbase
##Config app.config or web.config
```xml
<configSections>
    <section name="couchbase" type="Couchbase.Configuration.Client.Providers.CouchbaseClientSection, Couchbase.NetClient" />
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler,log4net" />
  </configSections>

  <appSettings>
    <add key="nhibernate:cache:bucket:name" value="test" />
  </appSettings>

  <couchbase>
    <servers>
      <add uri="http://localhost:8091"></add>
    </servers>
    <buckets>
      <add name="test"></add>
    </buckets>
  </couchbase>
```
##Config NHibernate.cfg.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<hibernate-configuration xmlns="urn:nhibernate-configuration-2.2">
  <session-factory>
    <property name="dialect">NHibernate.Dialect.MsSql2008Dialect</property>
    <property name="connection.provider">NHibernate.Connection.DriverConnectionProvider</property>
    <property name="connection.driver_class">NHibernate.Driver.SqlClientDriver</property>
    <property name="connection.connection_string">Password=test;Persist Security Info=True;User ID=test;Initial Catalog=yourDatabase;Data Source=localhost</property>
    <property name="current_session_context_class">web</property>
    <property name="command_timeout">0</property>
    <property name="query.substitutions">true 1, false 0, yes 'Y', no 'N'</property>
    <property name="default_schema">yourDatabaseSchema.dbo</property>
    <property name="cache.provider_class">NHibernate.Cache.Couchbase.CacheProvider,NHibernate.Cache.Couchbase</property>
    <property name="cache.use_second_level_cache">true</property>
    <property name="cache.use_query_cache">true</property>
    <property name="cache.region_prefix" >regionSql</property>
    <property name="cache.default_expiration" >60</property>
    
  </session-factory>
</hibernate-configuration>
```

##Code in Global.asax
```c
protected void Application_Start()
{
    Cache.Couchbase.Supports.ConnectionProvider.Start();
}
  ```      
