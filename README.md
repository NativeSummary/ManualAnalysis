# Manual Analysis Results

This repo contains Manual Analysis Results for the NativeSummary paper.

Initially, we intended to use a Google Docs tables to present our data, but it cannot collapse table blocks, and only Enterprise version support the code block. So we created a webpage for the data.

Open this link:

- [50 verified native related flows](https://nativesummary.github.io/ManualAnalysis/verified_flows.html)

The HTML file is intended to be a single self-contained webpage, you can also download and open it locally.

## An example of the manual analysis

Here we present an example of our manual analysis process.

1. First, We fill in the APK name, source and sink, native parts of the dataflow first.
   1. Here, the apk name: com.sgr_b2.compass_18.apk 
   2. Native part: `$f0 = staticinvoke <com.sgr_b2.compass.jni.CCJNI: float cmps_sanitize_lon(float)>($f0)`
2. Then, we search for the classname for source code. Or go to the F-Droid apk page, find the source code.
   1. Search "com.sgr_b2.compass" and find repo, search "cmps_sanitize_lon" to find the function
   2. Source code: https://bitbucket.org/alekseyt/compass/src/92a4381be7a51efc4d1939851f17b5d513a1bc52/jni/libcompass/utils.c?at=master#lines-11:14
3. Then, we find the Jimple IR, and check if the data flow is correct.
   - the dataflow propogate from the argument to the return value.

Full Flow:

```xml
         <Source Statement="$r3 = virtualinvoke $r1.&lt;java.text.DecimalFormat: java.lang.Number parse(java.lang.String)&gt;($r0)" Method="&lt;com.sgr_b2.compass.ui.d: float a(java.lang.String)&gt;">
          <AccessPath Value="$r3" Type="java.lang.Number" TaintSubFields="true" />
          <TaintPath>
            <PathElement Statement="$r3 = virtualinvoke $r1.&lt;java.text.DecimalFormat: java.lang.Number parse(java.lang.String)&gt;($r0)" Method="&lt;com.sgr_b2.compass.ui.d: float a(java.lang.String)&gt;">
              <AccessPath Value="$r3" Type="java.lang.Number" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="$f0 = virtualinvoke $r3.&lt;java.lang.Number: float floatValue()&gt;()" Method="&lt;com.sgr_b2.compass.ui.d: float a(java.lang.String)&gt;">
              <AccessPath Value="$f0" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="return $f0" Method="&lt;com.sgr_b2.compass.ui.d: float a(java.lang.String)&gt;">
              <AccessPath Value="$f1" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="$f1 = staticinvoke &lt;com.sgr_b2.compass.jni.k: float b(float)&gt;($f1)" Method="&lt;com.sgr_b2.compass.activities.AddEditActivity: void onClick(android.view.View)&gt;">
              <AccessPath Value="$f0" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="$f0 = staticinvoke &lt;com.sgr_b2.compass.jni.CCJNI: float cmps_sanitize_lon(float)&gt;($f0)" Method="&lt;com.sgr_b2.compass.jni.k: float b(float)&gt;">
              <AccessPath Value="$f0" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="return $f0" Method="&lt;com.sgr_b2.compass.jni.CCJNI: float cmps_sanitize_lon(float)&gt;">
              <AccessPath Value="$f0" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="return $f0" Method="&lt;com.sgr_b2.compass.jni.k: float b(float)&gt;">
              <AccessPath Value="$f1" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="specialinvoke r0.&lt;com.sgr_b2.compass.activities.AddEditActivity: void a(java.lang.String,float,float)&gt;($r4, $f0, $f1)" Method="&lt;com.sgr_b2.compass.activities.AddEditActivity: void onClick(android.view.View)&gt;">
              <AccessPath Value="$f1" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="specialinvoke $r5.&lt;com.sgr_b2.compass.a.d: void &lt;init&gt;(float,float,java.lang.String,int)&gt;($f0, $f1, $r1, $i0)" Method="&lt;com.sgr_b2.compass.activities.AddEditActivity: void a(java.lang.String,float,float)&gt;">
              <AccessPath Value="$f1" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="r0.&lt;com.sgr_b2.compass.a.d: float d&gt; = $f1" Method="&lt;com.sgr_b2.compass.a.d: void &lt;init&gt;(float,float,java.lang.String,int)&gt;">
              <AccessPath Value="r0" Type="com.sgr_b2.compass.a.d" TaintSubFields="true">
                <Fields>
                  <Field Value="&lt;com.sgr_b2.compass.a.d: float d&gt;" Type="float" />
                </Fields>
              </AccessPath>
            </PathElement>
            <PathElement Statement="return" Method="&lt;com.sgr_b2.compass.a.d: void &lt;init&gt;(float,float,java.lang.String,int)&gt;">
              <AccessPath Value="$r5" Type="com.sgr_b2.compass.a.d" TaintSubFields="true">
                <Fields>
                  <Field Value="&lt;com.sgr_b2.compass.a.d: float d&gt;" Type="float" />
                </Fields>
              </AccessPath>
            </PathElement>
            <PathElement Statement="virtualinvoke $r2.&lt;com.sgr_b2.compass.a.a: void b(com.sgr_b2.compass.a.d)&gt;($r5)" Method="&lt;com.sgr_b2.compass.activities.AddEditActivity: void a(java.lang.String,float,float)&gt;">
              <AccessPath Value="$r1" Type="com.sgr_b2.compass.a.d" TaintSubFields="true">
                <Fields>
                  <Field Value="&lt;com.sgr_b2.compass.a.d: float d&gt;" Type="float" />
                </Fields>
              </AccessPath>
            </PathElement>
            <PathElement Statement="$f0 = $r1.&lt;com.sgr_b2.compass.a.d: float d&gt;" Method="&lt;com.sgr_b2.compass.a.a: void b(com.sgr_b2.compass.a.d)&gt;">
              <AccessPath Value="$f0" Type="float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="$r3 = staticinvoke &lt;java.lang.Float: java.lang.Float valueOf(float)&gt;($f0)" Method="&lt;com.sgr_b2.compass.a.a: void b(com.sgr_b2.compass.a.d)&gt;">
              <AccessPath Value="$r3" Type="java.lang.Float" TaintSubFields="true" />
            </PathElement>
            <PathElement Statement="virtualinvoke $r2.&lt;android.content.ContentValues: void put(java.lang.String,java.lang.Float)&gt;(&quot;lon&quot;, $r3)" Method="&lt;com.sgr_b2.compass.a.a: void b(com.sgr_b2.compass.a.d)&gt;">
              <AccessPath Value="$r3" Type="java.lang.Float" TaintSubFields="true" />
            </PathElement>
          </TaintPath>
        </Source>
```


Source Code: 

```cpp
// https://bitbucket.org/alekseyt/compass/src/92a4381be7a51efc4d1939851f17b5d513a1bc52/jni/libcompass/utils.c?at=master#lines-11:14
float cmps_sanitize_lon(float lon) {
	return (lon < -180 ? -180 : (
	lon > 180 ? 180 : lon));
}
```

Jimple IR:

```cpp
public static float cmps_sanitize_lon(float)
    {
        float $p0;
        $p0 := @parameter0;
        return $p0;
    }
```
