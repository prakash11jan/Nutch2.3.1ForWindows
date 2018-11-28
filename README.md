# Nutch2.3.1ForWindows

1. download nutch2.3.1 provided from 
https://wiki.apache.org/nutch/Nutch2Tutorial
2. download Hbase too from above and unzip in cygwin path preferred(c:/cygwin64/home/Hbase). so that root and data directory will in cygwin path (refer hbase-site.xml)
3. add the following in nutch-site.xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

<property>
 <name>storage.data.store.class</name>
 <value>org.apache.gora.hbase.store.HBaseStore</value>
 <description>Default class for storing data</description>
</property>


<property>
  <name>file.content.limit</name>
  <value>-1</value>
  <description>The length limit for downloaded content using the file://
  protocol, in bytes. If this value is nonnegative (>=0), content longer
  than it will be truncated; otherwise, no truncation at all. Do not
  confuse this setting with the http.content.limit setting.
  </description>
</property>

<property>
  <name>ftp.content.limit</name>
  <value>-1</value>
  <!-- <value>65536</value> -->
  <description>The length limit for downloaded content, in bytes.
  If this value is nonnegative (>=0), content longer than it will be truncated;
  otherwise, no truncation at all.
  Caution: classical ftp RFCs never defines partial transfer and, in fact,
  some ftp servers out there do not handle client side forced close-down very
  well. Our implementation tries its best to handle such situations smoothly.
  </description>
</property>

<property>
  <name>http.content.limit</name>
  <value>-1</value>
  <description>The length limit for downloaded content using the http://
  protocol, in bytes. If this value is nonnegative (>=0), content longer
  than it will be truncated; otherwise, no truncation at all. Do not
  confuse this setting with the file.content.limit setting.
  </description>
</property>




<property>
 <name>http.agent.name</name>
 <value>MyBot</value>
 <description>MUST NOT be empty. The advertised version will have Nutch appended.</description>
</property>
<property>
 <name>http.robots.agents</name>
 <value>MyBot,*</value>
 <description>The agent strings we'll look for in robots.txt files,
 comma-separated, in decreasing order of precedence. You should
 put the value of http.agent.name as the first agent name, and keep the
 default * at the end of the list. E.g.: BlurflDev,Blurfl,*. If you don't, your logfile will be full of warnings.
 </description>
</property>
<property>
 <name>fetcher.store.content</name>
 <value>true</value>
 <description>If true, fetcher will store content. Helpful on the getting-started stage, as you can recover failed steps, but may cause performance problems on larger crawls.</description>
</property>

<property>
 <name>fetcher.max.crawl.delay</name>
 <value>-1</value>
 <description>
 If the Crawl-Delay in robots.txt is set to greater than this value (in
 seconds) then the fetcher will skip this page, generating an error report. If set to -1 the fetcher will never skip such pages and will wait the amount of time retrieved from robots.txt Crawl-Delay, however long that might be.
 </description>
</property>

<!-- Applicable plugins-->
 <property>
 <name>plugin.includes</name>
 <value>protocol-httpclient|urlfilter-regex|parse-(html|tika|metatags)|index-(basic|anchor|metadata)|query-(basic|site|url)|response-(json|xml)|summary-basic|scoring-opic|indexer-solr|urlnormalizer-(pass|regex|basic)</value>
<description> At the very least, I needed to add the parse-html, urlfilter-regex, and the indexer-solr.
</description>
 </property>

</configuration>
 4. Add the following in ivy.xml
 
 <configuration>
  <property>
    <name>hbase.rootdir</name>
    <value>file:///home/PXR26/hbase</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/home/PXR26/zookeeper</value>
  </property>
  <property>
    <name>hbase.unsafe.stream.capability.enforce</name>
    <value>false</value>
    <description>
      Controls whether HBase will check for stream capabilities (hflush/hsync).

      Disable this if you intend to run on LocalFileSystem, denoted by a rootdir
      with the 'file://' scheme, but be mindful of the NOTE below.

      WARNING: Setting this to false blinds you to potential data loss and
      inconsistent system state in the event of process and/or node failures. If
      HBase is complaining of an inability to use hsync or hflush it's most
      likely not a false positive.
    </description>
  </property>
</configuration>

5. add the foolwing in gora.properties 
gora.datastore.default=org.apache.gora.hbase.store.HBaseStore

6. now start hbase server using window command. navigate  to bin directory of Habse and provide start-hbase.cmd


 7. also we need to download if we did not set hadoop home HADOOP_HOME = C:\cygwin64\home\hadoop\hadoop-2.7.1 PATH = %HADOOP_HOME%\bin;
