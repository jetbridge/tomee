index-group=Unrevised
type=page
status=published
~~~~~~
<div class="section">
<h2>tomee:help<a name="tomee:help"></a></h2>
      
<p><b>Full name</b>:</p>
      
<p>org.apache.openejb.maven:tomee-maven-plugin[:Current Version]:help</p>
      
<p><b>Description</b>:</p>
      
<div>Display help information on tomee-maven-plugin.<br />
Call <tt>mvn tomee:help -Ddetail=true
-Dgoal=&lt;goal-name&gt;</tt> to display parameter details.</div>
      
<p><b>Attributes</b>:</p>
      
<ul>
        
<li>The goal is thread-safe and supports parallel builds.</li>
      </ul>
      
<div class="section">
<h3>Optional Parameters<a name="Optional_Parameters"></a></h3>
        
<table class="mdtable">
          
<tr class="a">
            
<th>Name</th>
            
<th>Type</th>
            
<th>Since</th>
            
<th>Description</th>
          </tr>
          
<tr class="b">
            
<td><b><a href="#detail">detail</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>If <tt>true</tt>, display all settable properties for each
goal.<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>detail</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#goal">goal</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>The name of the goal for which to show help. If unspecified, all
goals will be displayed.<br /><b>User property is</b>: <tt>goal</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#indentSize">indentSize</a></b></td>
            
<td><tt>int</tt></td>
            
<td><tt>-</tt></td>
            
<td>The number of spaces per indentation level, should be positive.<br /><b>Default value is</b>: <tt>2</tt>.<br /><b>User property is</b>: <tt>indentSize</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#lineLength">lineLength</a></b></td>
            
<td><tt>int</tt></td>
            
<td><tt>-</tt></td>
            
<td>The maximum length of a display line, should be positive.<br /><b>Default value is</b>: <tt>80</tt>.<br /><b>User property is</b>: <tt>lineLength</tt>.</td>
          </tr>
        </table>
      </div>
      
<div class="section">
<h3>Parameter Details<a name="Parameter_Details"></a></h3>
        
<p><b><a name="detail">detail</a>:</b></p>
        
<div>If <tt>true</tt>, display all settable properties for each
goal.</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>detail</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="goal">goal</a>:</b></p>
        
<div>The name of the goal for which to show help. If unspecified, all
goals will be displayed.</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>goal</tt></li>
        </ul><hr />
<p><b><a name="indentSize">indentSize</a>:</b></p>
        
<div>The number of spaces per indentation level, should be positive.</div>
        
<ul>
          
<li><b>Type</b>: <tt>int</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>indentSize</tt></li>
          
<li><b>Default</b>: <tt>2</tt></li>
        </ul><hr />
<p><b><a name="lineLength">lineLength</a>:</b></p>
        
<div>The maximum length of a display line, should be positive.</div>
        
<ul>
          
<li><b>Type</b>: <tt>int</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>lineLength</tt></li>
          
<li><b>Default</b>: <tt>80</tt></li>
        </ul>
      </div>
    </div>