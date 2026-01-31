<h1 align="center">SQL Security Analysis: Incident Investigation & Asset Management</h1>

<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; border-left: 5px solid #007acc; color: #ffffff;">
  <strong>Resources Used:</strong>
  <ul>
    <li><a href="https://mariadb.org/" style="color: #4db8ff;">MariaDB (SQL) </a></li>
    <li><a href="https://www.skills.google/" style="color: #4db8ff;">Google Skills (Qwiklabs) </a></li>
  </ul>
</div>

<h2>Why Security Filtering is Essential?</h2>

<p>In cybersecurity, data is only useful if it is actionable. My organization is working to make their system more secure, and it is my job to ensure the system is safe by investigating all potential security issues. Organizations generate massive amounts of log data, and without precise filtering, critical threats like unauthorized after-hours access or brute-force attempts can easily be missed. By using SQL filters, I can isolate high-risk activity from the noise of daily logs and ensure that security patches are applied only to the specific departments or devices that need them, preventing operational downtime and maintaining a strong defense posture.</p>

<h2>1. After-Hours Incident Investigation</h2> 

<p><strong>Scenario:</strong> A potential security incident occurred after 18:00, which is outside of standard business hours. To maintain system integrity, I had to investigate every failed login attempt during this window to pinpoint suspicious activity.</p> 

<p><strong>The Filter:</strong> I selected all data from the <code>log_in_attempts</code> table and used a <code>WHERE</code> clause with an <code>AND</code> operator to isolate login attempts occurring after 18:00 (<code>login_time > '18:00'</code>) that were also unsuccessful (<code>success = FALSE</code>).</p>

<p align="center">
  <img width="795" height="229" alt="Screenshot 2026-01-30 175532" src="https://github.com/user-attachments/assets/7cb4721e-9020-4bdc-a99c-27a16bc9152c" />
  <br />
  <em>Figure 1: Querying for unsuccessful logins after 18:00 using AND logic.</em>
</p>


<h2>2. Specific Date Telemetry Audit</h2> 

<p><strong>Scenario:</strong> A suspicious event occurred on 2022-05-09. To assess the full impact, I needed to review all login activity from that day and the previous day, 2022-05-08, to capture all relevant telemetry.</p> 

<p><strong>The Filter:</strong> I used an <code>OR</code> operator within the <code>WHERE</code> clause to capture <code>login_date</code> entries for either '2022-05-09' or '2022-05-08'. This allowed me to include attempts from both dates in a single result set for efficient auditing.</p>

<p align="center">
  <img width="803" height="228" alt="Screenshot 2026-01-30 175609" src="https://github.com/user-attachments/assets/b330bbb4-e1f6-4249-a84f-ec9ab1f0b67d" />
  <br />
  <em>Figure 2: Using the OR operator to retrieve logs from multiple specific dates.</em>
</p>

<h2>3. Geographic Risk Filtering</h2> 

<p><strong>Scenario:</strong> My investigation of login attempts suggested potential security issues originating from locations outside of Mexico.</p> 

<p><strong>The Filter:</strong> Because the dataset represents Mexico as both 'MEX' and 'MEXICO', I used the <code>NOT</code> operator combined with the <code>LIKE 'MEX%'</code> pattern. The percentage sign wildcard allowed the query to exclude all variations of the country, successfully isolating international login attempts for further review.</p>

<p align="center">
  <img width="798" height="228" alt="Screenshot 2026-01-30 175649" src="https://github.com/user-attachments/assets/119f018a-610d-4c30-aacb-55b55dc92ef4" />
  <br />
  <em>Figure 3: Implementing NOT and LIKE operators with wildcards for regional exclusion.</em>
</p>

<h2>4. Targeted Asset Auditing</h2> 

<p><strong>Scenario:</strong> My team needed to update the computers for certain employees in the Marketing department who work in the East building. I had to identify exactly which machines required these updates.</p> 

<p><strong>The Filter:</strong> I used an <code>AND</code> operator to combine two conditions from the <code>employees</code> table: <code>department = 'Marketing'</code> and <code>office LIKE 'East%'</code>. The wildcard allowed me to capture all office numbers within the East building.</p>

<p align="center">
  <img width="601" height="227" alt="Screenshot 2026-01-30 180144" src="https://github.com/user-attachments/assets/940424fe-536a-464b-a4eb-11e6d8d6169c" />
  <br /> 
  <em>Figure 4: Combining department filters with office location wildcards.</em>
</p>

<h2>5. Cross-Departmental Security Updates</h2> 

<p><strong>Scenario:</strong> A different security update was required specifically for employees in the Finance and Sales departments.</p> 

<p><strong>The Filter:</strong> I used the <code>OR</code> operator instead of <code>AND</code> to retrieve records for any employee belonging to either <code>department = 'Finance'</code> or <code>department = 'Sales'</code>. This ensured both groups were captured in one list for the update lifecycle.</p>

<p align="center">
  <img width="624" height="223" alt="Screenshot 2026-01-30 180302" src="https://github.com/user-attachments/assets/e3735501-b4e9-4ee1-9817-eea3b1a2ffc5" />
  <br />
  <em>Figure 5: Retrieving employee records for multiple departments simultaneously.</em>
</p>

<h2>6. Organizational-Wide Security Lifecycle</h2> 

<p><strong>Scenario:</strong> One final security update was required for all organizational employees except those in the Information Technology department.</p> 

<p><strong>The Filter:</strong> I used a <code>WHERE</code> clause with the <code>NOT</code> operator on the department column to exclude the Information Technology department from the results. This isolated the machine information for all non-technical staff to ensure comprehensive patch coverage.</p>

<p align="center">
  <img width="680" height="226" alt="Screenshot 2026-01-30 180346" src="https://github.com/user-attachments/assets/90e95b79-1c03-488c-8455-fd9faab77729" />
  <br />
  <em>Figure 6: Using the NOT operator to isolate non-technical staff for security patching.</em>
</p>

<h2>Technical Skills Demonstrated:</h2>

<ul> <li><strong>Database Querying:</strong> Selecting and analyzing security data from the <code>log_in_attempts</code> and <code>employees</code> tables.</li> 
  <li><strong>Logical Filtering:</strong> Applying <code>AND</code>, <code>OR</code>, and <code>NOT</code> operators to extract specific security telemetry.</li> 
  <li><strong>Pattern Recognition:</strong> Implementing <code>LIKE</code> and the <code>%</code> wildcard to manage inconsistent data entries and location strings.</li> </ul>

<h1>Final Reflection</h1> 

<p>I applied SQL filters to extract precise information regarding login attempts and employee assets to support organizational security. This project highlights the ability to transform raw database logs into actionable intelligence for incident investigation and lifecycle management. By mastering these filters, I can efficiently identify suspicious activity and manage large-scale hardware updates across diverse departments. This structural approach ensures that security resources are focused on high-risk areas while maintaining operational continuity.</p>

<p>Thank you for reviewing my project and for your consideration of my work.</p>
