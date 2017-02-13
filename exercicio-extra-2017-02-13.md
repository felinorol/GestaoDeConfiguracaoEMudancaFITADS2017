Apache
-*- coding: utf-8 -*-

Changes with Apache 2.4.25

  *) Fix some build issues related to various modules.
     [Rainer Jung]

Changes with Apache 2.4.24

  *) SECURITY: CVE-2016-8740 (cve.mitre.org)
     mod_http2: Mitigate DoS memory exhaustion via endless
     CONTINUATION frames.
     [Naveen Tiwari <naveen.tiwari@asu.edu> and CDF/SEFCOM at Arizona State
     University, Stefan Eissing]

  *) SECURITY: CVE-2016-5387 (cve.mitre.org)
     core: Mitigate [f]cgi "httpoxy" issues.
     [Dominic Scheirlinck <dominic vendhq.com>, Yann Ylavic]

  *) SECURITY: CVE-2016-2161 (cve.mitre.org)
     mod_auth_digest: Prevent segfaults during client entry allocation when
     the shared memory space is exhausted.
     [Maksim Malyutin <m.malyutin dsec.ru>, Eric Covener, Jacob Champion]

  *) SECURITY: CVE-2016-0736 (cve.mitre.org)
     mod_session_crypto: Authenticate the session data/cookie with a
     MAC (SipHash) to prevent deciphering or tampering with a padding
     oracle attack.  [Yann Ylavic, Colm MacCarthaigh]

  *) SECURITY: CVE-2016-8743 (cve.mitre.org)
     Enforce HTTP request grammar corresponding to RFC7230 for request lines
     and request headers, to prevent response splitting and cache pollution by
     malicious clients or downstream proxies. [William Rowe, Stefan Fritsch]

  *) Validate HTTP response header grammar defined by RFC7230, resulting
     in a 500 error in the event that invalid response header contents are
     detected when serving the response, to avoid response splitting and cache
     pollution by malicious clients, upstream servers or faulty modules.
     [Stefan Fritsch, Eric Covener, Yann Ylavic]

  *) mod_rewrite: Limit runaway memory use by short circuiting some kinds of
     looping RewriteRules when the local path significantly exceeds 
     LimitRequestLine.  PR 60478. [Jeff Wheelhouse <apache wheelhouse.org>]

  *) mod_ratelimit: Allow for initial "burst" amount at full speed before
     throttling: PR 60145 [Andy Valencia <ajv-etradanalhos vsta.org>,
     Jim Jagielski]

  *) mod_socache_memcache: Provide memcache stats to mod_status.
     [Jim Jagielski]

  *) http_filters: Fix potential looping in new check_headers() due to new
     pattern of ap_die() from http header filter. Explicitly clear the
     previous headers and body.

  *) core: Drop Content-Length header and message-body from HTTP 204 responses.
     PR 51350 [Luca Toscano]

  *) mod_proxy: Honor a server scoped ProxyPass exception when ProxyPass is
     configured in <Location>, like in 2.2. PR 60458.
     [Eric Covener]

  *) mod_lua: Fix default value of LuaInherit directive. It should be 
     'parent-first' instead of 'none', as per documentation.  PR 60419
     [Christophe Jaillet]

  *) core: New directive HttpProtocolOptions to control httpd enforcement
     of various RFC7230 requirements. [Stefan Fritsch, William Rowe]

  *) core: Permit unencoded ';' characters to appear in proxy requests and
     Location: response headers. Corresponds to modern browser behavior.
     [William Rowe]

  *) core: ap_rgetline_core now pulls from r->proto_input_filters.

  *) core: Correctly parse an IPv6 literal host specification in an absolute
     URL in the request line. [Stefan Fritsch]

  *) core: New directive RegisterHttpMethod for registering non-standard
     HTTP methods. [Stefan Fritsch]

  *) mod_socache_memcache: Pass expiration time through to memcached.
     [Faidon Liambotis <paravoid debian.org>, Joe Orton]

  *) mod_cache: Use the actual URI path and query-string for identifying the
     cached entity (key), such that rewrites are taken into account when
     running afterwards (CacheQuickHandler off).  PR 21935.  [Yann Ylavic]

  *) mod_http2: new directive 'H2EarlyHints' to enable sending of HTTP status
     103 interim responses. Disabled by default. [Stefan Eissing]
     
  *) mod_ssl: Fix quick renegotiation (OptRenegotiaton) with no intermediate
     in the client certificate chain.  PR 55786.  [Yann Ylavic]

  *) event: Allow to use the whole allocated scoreboard (up to ServerLimit
     slots) to avoid scoreboard full errors when some processes are finishing
     gracefully. Also, make gracefully finishing processes close all
     keep-alive connections. PR 53555. [Stefan Fritsch]

  *) mpm_event: Don't take over scoreboard slots from gracefully finishing
     threads. [Stefan Fritsch]

  *) mpm_event: Free memory earlier when shutting down processes.
     [Stefan Fritsch]

  *) mod_status: Display the process slot number in the async connection
     overview. [Stefan Fritsch]

  *) mod_dir: Responses that go through "FallbackResource" might appear to
     hang due to unterminated chunked encoding. PR58292. [Eric Covener]

  *) mod_dav: Fix a potential cause of unbounded memory usage or incorrect
     behavior in a routine that sends <DAV:response>'s to the output filters.
     [Evgeny Kotkov]

  *) mod_http2: new directive 'H2PushResource' to enable early pushes before 
     processing of the main request starts. Resources are announced to the 
     client in Link headers on a 103 early hint response. 
     All responses with status code <400 are inspected for Link header and
     trigger pushes accordingly. 304 still does prevent pushes.
     'H2PushResource' can mark resources as 'critical' which gives them higher
     priority than the main resource. This leads to preferred scheduling for
     processing and, when content is available, will send it first. 'critical'
     is also recognized on Link headers. [Stefan Eissing]
     
  *) mod_proxy_http2: uris in Link headers are now mapped back to a suitable
     local url when available. Relative uris with an absolute path are mapped
     as well. This makes reverse proxy mapping available for resources
     announced in this header. 
     With 103 interim responses being forwarded to the main client connection,
     this effectively allows early pushing of resources by a reverse proxied
     backend server. [Stefan Eissing]
     
  *) mod_proxy_http2: adding support for newly proposed 103 status code.
     [Stefan Eissing]
     
  *) mpm_unix: Apache fails to start if previously crashed then restarted with
     the same PID (e.g. in container).  PR 60261.
     [Val <valentin.bremond gmail.com>, Yann Ylavic]

  *) mod_http2: unannounced and multiple interim responses (status code < 200)
     are parsed and forwarded to client until a final response arrives.
     [Stefan Eissing]
  
  *) mod_proxy_http2: improved robustness when main connection is closed early
     by resetting all ongoing streams against the backend.
     [Stefan Eissing]
  
  *) mod_http2: allocators from slave connections are released earlier,
     resulting in less overall memory use on busy, long lived connections.
     [Stefan Eissing]
     
  *) mod_remoteip: Pick up where we left off during a subrequest rather
     than running with the modified XFF but original TCP address.
     PR 49839/PR 60251

  *) http: Respond with "408 Request Timeout" when a timeout occurs while
     reading the request body.  [Yann Ylavic]

  *) mod_http2: connection shutdown revisited: corrected edge cases on
     shutting down ongoing streams, changed log warnings to be less noisy
     when waiting on long running tasks. [Stefan Eissing]

  *) mod_http2: changed all AP_DEBUG_ASSERT to ap_assert to have them 
     available also in normal deployments. [Stefan Eissing]

  *) mod_http2/mod_proxy_http2: 100-continue handling now properly implemented
     up to the backend. Reused HTTP/2 proxy connections with more than a second
     not used will block request bodies until a PING answer is received.
     Requests headers are not delayed by this, since they are repeatable in
     case of failure. This greatly increases robustness, especially with
     busy server and/or low keepalive connections. [Stefan Eissing]
     
  *) mod_proxy_http2: fixed duplicate symbols with mod_http2.
     [Stefan Eissing]
  
  *) mod_http2: rewrite of how responses and trailers are transferred between
     master and slave connection. Reduction of internal states for tasks
     and streams, stability. Heuristic id generation for slave connections
     to better keep promise of connection ids unique at given point int time.
     Fix for mod_cgid interop in high load situtations. 
     Fix for handling of incoming trailers when no request body is sent.
     [Stefan Eissing]
  
  *) mod_http2: fix suspended handling for streams. Output could become
     blocked in rare cases. [Stefan Eissing]

  *) mpm_winnt: Prevent a denial of service when the 'data' AcceptFilter is in
     use by replacing it with the 'connect' filter. PR 59970. [Jacob Champion]

  *) mod_cgid: Resolve a case where a short CGI response causes a subsequent
     CGI to be killed prematurely, resulting in a truncated subsequent
     response. [Eric Covener]

  *) mod_proxy_hcheck: Set health check URI and expression correctly for health
     check worker. PR 60038 [zdeno <zdeno@scnet.sk>]

  *) mod_http2: if configured with nghttp2 1.14.0 and onward, invalid request
     headers will immediately reset the stream with a PROTOCOL error. Feature
     logged by module on startup as 'INVHD' in info message.
     [Stefan Eissing]
     
  *) mod_http2: fixed handling of stream buffers during shutdown.
     [Stefan Eissing]
     
  *) mod_reqtimeout: Fix body timeout disabling for CONNECT requests to avoid
     triggering mod_proxy_connect's AH01018 once the tunnel is established.
     [Yann Ylavic]

  *) ab: Set the Server Name Indication (SNI) extension on outgoing TLS
     connections (unless -I is specified), according to the Host header (if
     any) or the requested URL's hostname otherwise.  [Yann Ylavic]

  *) mod_proxy_fcgi: avoid loops when ProxyErrorOverride is enabled
     and the error documents are proxied. PR 55415. [Luca Toscano]

  *) mod_proxy_fcgi: read the whole FCGI response even when the content
     has not been modified (HTTP 304) or in case of a precondition failure
     (HTTP 412) to avoid subsequent bogus reads and confusing
     error messages logged. [Luca Toscano]

  *) mod_http2: h2 status resource follows latest draft, see
     http://www.ietf.org/id/draft-benfield-http2-debug-state-01.txt
     [Stefan Eissing]
     
  *) mod_http2: handling graceful shutdown gracefully, e.g. handling existing
     streams to the end. [Stefan Eissing]
  
  *) mod_proxy_{http,ajp,fcgi}: don't reuse backend connections with data
     available before the request is sent.  PR 57832.  [Yann Ylavic]

  *) mod_proxy_balancer: Prevent redirect loops between workers within a
     balancer by limiting the number of redirects to the number balancer
     members. PR 59864 [Ruediger Pluem]

  *) mod_proxy: Correctly consider error response codes by the backend when
     processing failonstatus. PR 59869 [Ruediger Pluem]

  *) mod_dav: Add dav_get_provider_name() function to obtain the name
     of the provider from mod_dav.  [Graham Leggett]

  *) mod_dav: Add support for childtags to dav_error.
     [Jari Urpalainen <jari.urpalainen nokia.com>]

  *) mod_proxy_fcgi: Fix 2.4.23 breakage for mod_rewrite per-dir and query 
     string showing up in SCRIPT_FILENAME. PR59815

  *) mod_include: Fix a potential memory misuse while evaluating expressions.
     PR59844. [Eric Covener]

  *) mod_http2: new H2CopyFiles directive that changes treatment of file
     handles in responses. Necessary in order to fix broken lifetime handling
     in modules such as mod_wsgi.
  
  *) mod_http2: removing timeouts on master connection while requests are
     being processed. Requests may timeout, but the master only times out when
     no more requests are active. [Stefan Eissing]
     
  *) mod_http2: fixes connection flush when answering SETTINGS without any
     stream open. [Moto Ishizawa <@summerwind>, Stefan Eissing]
     


  [Apache 2.3.0-dev includes those bug fixes and changes with the
   Apache 2.2.xx tree as documented, and except as noted, below.]

Changes with Apache 2.2.x and later:

  *) http://svn.apache.org/viewvc/httpd/httpd/branches/2.2.x/CHANGES?view=markup

Changes with Apache 2.0.x and later:

  *) http://svn.apache.org/viewvc/httpd/httpd/branches/2.0.x/CHANGES?view=markup

Fonte: https://httpd.apache.org/  ;  http://www.apache.org/dist/httpd/CHANGES_2.4.25

---------------------------------------------------------------------------------------------------------------------------
Tomcat

Apache Tomcat Versions

Apache Tomcat® is an open source software implementation of the Java Servlet and JavaServer Pages technologies. Different versions of Apache Tomcat are available for different versions of the Servlet and JSP specifications. The mapping between the specifications and the respective Apache Tomcat versions is:

Servlet Spec	JSP Spec	EL Spec	WebSocket Spec	JASPIC Spec	Apache Tomcat Version	Latest Released Version	Supported Java Versions
4.0	TBD (2.4?)	TBD (3.1?)	TBD (1.2?)	1.1	9.0.x	9.0.0.M17 (alpha)	8 and later
3.1	2.3	3.0	1.1	1.1	8.5.x	8.5.11	7 and later
3.1	2.3	3.0	1.1	N/A	8.0.x (superseded)	8.0.41 (superseded)	7 and later
3.0	2.2	2.2	1.1	N/A	7.0.x	7.0.75	6 and later
(7 and later for WebSocket)
2.5	2.1	2.1	N/A	N/A	6.0.x	6.0.48	5 and later
2.4	2.0	N/A	N/A	N/A	5.5.x (archived)	5.5.36 (archived)	1.4 and later
2.3	1.2	N/A	N/A	N/A	4.1.x (archived)	4.1.40 (archived)	1.3 and later
2.2	1.1	N/A	N/A	N/A	3.3.x (archived)	3.3.2 (archived)	1.1 and later
Each version of Tomcat is supported for any stable Java release that meets the requirements of the final column in the table above.

Tomcat should also work on any Java early access build that meets the requirements of the final column in the table above. For example, users were successfully running Tomcat 8 on Java 8 many months before the first stable Java 8 release. However, users of early access builds should be aware of the following:

It is not unusual for the initial early access builds to contain bugs that can cause problems for web applications running on Tomcat.
If the new Java version introduces new language features then the default JSP compiler may not support them immediately. Switching the JSP compiler to javac may enable these new language features to be used in JSPs.
If you do discover an problem using a Java early access build, please ask for help. The Tomcat user's mailing list is probably the best place to start.
The releases are described in more detail below to help you determine which one is right for you. More details about each release can be found in the associated release notes.

Please note that although we offer downloads and documentation of older releases, such as Apache Tomcat 6.x, we strongly encourage users to use the latest stable version of Apache Tomcat whenever possible. We recognize that upgrading across major versions may not be a trivial task, and some support is still offered on the mailing list for users of old versions. However, because of the community-driven support approach, the older your version, fewer people will be interested or able to support you.

Alpha / Beta / Stable

When voting for a release, reviewers specify the stability level that they consider the release has reached. Initial releases of a new major version typically process from Alpha, through Beta to Stable over a period of several months. However, the Stable level is only available once the Java specifications the release implements have been finalised. This means a release that in all other respects is considered stable, may still be labelled as Beta if the specifications are not final.

The download pages will always show the latest stable release and any newer Alpha or Beta release if one exists. Alpha and beta releases are always clearly marked on the download pages.

Stability is a subjective judgement and you should always read carefully the release notes for any version you intend to make use of. If you are an early adopter of a release, we would love to hear your opinion about its stability as part of the vote: it takes place on the development mailing list.

Alpha releases may contain large amounts of untested/missing functionality required by the specification and/or significant bugs and are not expected to run stably for any length of time.

Beta releases may contain some untested functionality and/or a number of relatively minor bugs. Beta releases are not expected to run stably.

Stable releases may contain a small number of relatively minor bugs. Stable releases are intended for production use and are expected to run stably for extended periods of time.

Apache Tomcat 9.x

Apache Tomcat 9.x is the current focus of development, it builds on Tomcat 8.0.x and implements the current draft of the Servlet 4.0 specification and will also implement the JSP 2.4?, EL 3.1?, WebSocket 1.2? and JASPIC 1.1 specifications once work starts on updating those specifications for Java EE 8. In addition to this, it includes the following significant improvements:

Adds support for HTTP/2 (requires the APR/native library)
Adds support for TLS virtual hosting
Adds support for using OpenSSL for TLS support with the JSSE connectors (NIO and NIO2)
Apache Tomcat 8.x

Apache Tomcat 8.x builds on Tomcat 7.0.x and implements the Servlet 3.1, JSP 2.3, EL 3.0 and WebSocket 1.1 specifications. In addition to that, it includes the following significant improvements:

A single, common resources implementation to replace the multiple resource extension features provided in earlier versions.
Apache Tomcat 8.5.x supports the same Servlet, JSP, EL, and WebSocket Specification versions as Apache Tomcat 8.0.x. In addition to that, it also implements the JASPIC 1.1 specification. There are significant changes in many areas under the hood, resulting in improved performance, stability, and total cost of ownership. Please refer to the Apache Tomcat 8.5 Changelog for details.

Apache Tomcat 7.x

Apache Tomcat 7.x builds upon the improvements made in Tomcat 6.0.x and implements the Servlet 3.0, JSP 2.2, EL 2.2 and WebSocket 1.1 specifications. In addition to that, it includes the following improvements:

Web application memory leak detection and prevention
Improved security for the Manager and Host Manager applications
Generic CSRF protection
Support for including external content directly in a web application
Refactoring (connectors, lifecycle) and lots of internal code clean-up
Apache Tomcat 6.x

Apache Tomcat 6.x builds upon the improvements made in Tomcat 5.5.x and implements the Servlet 2.5 and JSP 2.1 specifications. In addition to that, it includes the following improvements:

Memory usage optimizations
Advanced IO capabilities
Refactored clustering
Users of Tomcat 6 should be aware that Tomcat 6 has now reached end of life. Users of Tomcat 6.x should upgrade to Tomcat 7.x or later.

Apache Tomcat 5.x

Apache Tomcat 5.x is available for download from the archives.

Apache Tomcat 5.5.x supports the same Servlet and JSP Specification versions as Apache Tomcat 5.0.x. There are significant changes in many areas under the hood, resulting in improved performance, stability, and total cost of ownership. Please refer to the Apache Tomcat 5.5 Changelog for details.

Apache Tomcat 5.0.x improves on Apache Tomcat 4.1 in many ways, including:

Performance optimizations and reduced garbage collection
Refactored application deployer, with an optional standalone deployer allowing validation and compilation of a web application before putting it in production
Complete server monitoring using JMX and the manager web application
Scalability and reliability enhancements
Improved Taglibs handling, including advanced pooling and tag plugins
Improved platform integration, with native Windows and Unix wrappers
Embedding using JMX
Enhanced Security Manager support
Integrated session clustering
Expanded documentation
Apache Tomcat 4.x

Apache Tomcat 4.x is available for download from the archives.

Apache Tomcat 4.x implements a new servlet container (called Catalina) that is based on completely new architecture. The 4.x releases implement the Servlet 2.3 and JSP 1.2 specifications.

Apache Tomcat 4.1.x is a refactoring of Apache Tomcat 4.0.x, and contains significant enhancements, including:

JMX based administration features
JSP and Struts based administration web application
New Coyote connector (HTTP/1.1, AJP 1.3 and JNI support)
Rewritten Jasper JSP page compiler
Performance and memory efficiency improvements
Enhanced manager application support for integration with development tools
Custom Ant tasks to interact with the manager application directly from build.xml scripts
Apache Tomcat 4.0.x. Apache Tomcat 4.0.6 is the old production quality release. The 4.0 servlet container (Catalina) has been developed from the ground up for flexibility and performance. Version 4.0 implements the final released versions of the Servlet 2.3 and JSP 1.2 specifications. As required by the specifications, Apache Tomcat 4.0 also supports web applications built for the Servlet 2.2 and JSP 1.1 specifications with no changes.

Apache Tomcat 3.x

Apache Tomcat 3.x is available for download from the archives.

Version 3.3 is the current production quality release for the Servlet 2.2 and JSP 1.1 specifications. Apache Tomcat 3.3 is the latest continuation of the Apache Tomcat 3.x architecture; it is more advanced then 3.2.4, which is the 'old' production quality release.
Version 3.2.4 is the 'old' production quality release and is now in maintenance only mode.
Version 3.1.1 is a legacy release.
All Apache Tomcat 3.x releases trace their heritage back to the original Servlet and JSP implementations that Sun donated to the Apache Software Foundation. The 3.x versions all implement the Servlet 2.2 and JSP 1.1 specifications.

Apache Tomcat 3.3.x. Version 3.3.2 is the current production quality release. It continues the refactoring that was begun in version 3.2 and carries it to its logical conclusion. Version 3.3 provides a much more modular design and allows the servlet container to be customized by adding and removing modules that control the processing of servlet requests. This version also contains many performance improvements.

Apache Tomcat 3.2.x. Version 3.2 added few new features since 3.1; the major effort was a refactoring of the internals to improve performance and stability. The 3.2.1 release, like 3.1.1, was a security patch. Version 3.2.2 fixed a large number of bugs and all known specification compliance issues. Version 3.2.3 was a security update that closes a serious security hole. Version 3.2.4 is a minor bug fix release. All users of Apache Tomcat versions prior to 3.2.3 should upgrade as soon as possible. With the exception of fixes for critical security related bugs, development on the Apache Tomcat 3.2.x branch has stopped.

Apache Tomcat 3.1.x. The 3.1 release contained several improvements over Apache Tomcat 3.0, including servlet reloading, WAR file support and added connectors for the IIS and Netscape web servers. The latest maintenance release, 3.1.1, contained fixes for security problems. There is no active development ongoing for Apache Tomcat 3.1.x. Users of Apache Tomcat 3.1 should update to 3.1.1 to close the security holes and they are strongly encouraged to migrate to the current production release, Apache Tomcat 3.3.

Apache Tomcat 3.0.x. Initial Apache Tomcat release.

Fonte: http://tomcat.apache.org    ;   http://tomcat.apache.org/whichversion.html


---------------------------------------------------------------------------------------------------------------------------
Eclipse


Hudson CI Server Releases - Change Log

This page lists the enhancements, bug-fixes and other changes relating to each Hudson release

Hudson 3.3.3.
Hudson 3.3.3 is a security patch-set release with no new features. Please report any issues at bugs.eclipse.org

Security Fixes
Hudson CLI Lockdown

The use of the Hudson Command line Interface is now disabled by default and we recommend that it not be re-enabled unless Hudson is running inside of a controlled environment. 
An option is available on the main Hudson settings screen to explicitly enable the CLI should it be required.

Bug Fixes
The following issues track this security feature.

Bug 483533 - Add security advisory to the Hudson CLI screen
Bug 483532 - Lock down the Hudson CLI by default
Hudson 3.3.2.
Hudson 3.3.2 is a security related patch-set release with no new features. Please report any issues at bugs.eclipse.org

Security Fixes
CVE-2015-8031 - Hudson XML External Entity Injection

This patch addresses a critical vulnerability within the Hudson XML API which allows remote access to potentially sensitive information on the filesystem of the Hudson master server. This vulnerability exists in all versions of Hudson prior to this 3.3.2 release and as such we recommend that users of Hudson adopt this patch as soon as possible.

Credits

The Hudson team would like to thank Luca Carettoni, Fabian Beterke and Tushar Dalvi from LinkedIn for their work in uncovering and reporting this vulnerability.

Bug Fixes
Whilst rolling this security patch we took the opportunity to address some minor issues that have been reported on 3.3.1 at the same time.

Bug 479681 - Ant not executing on slave (specifically Linux)
Bug 480396 - Maven configuration page is unusable with Hudson 3.3.1
Bug 480529 - Console does not scroll to the bottom of the log (3.3.1)
Hudson 3.3.1.
Hudson 3.3.1 is a patch-set release with no new features. Please report any issues at bugs.eclipse.org

Bug Fixes
Bug 456954 - Undeploy/deploy Hudson WAR leaks Tomcat PermGen
Bug 407348 - httpListenAddress parameter is not honored
Bug 427509 - Build trigger not saved in upstream job
Bug 455838 - hudson.FilePathTest error on Windows 7
Bug 462021 - Multi-configuration job does not show proper job statuses
Bug 472217 - Proxy settings not saved
Bug 474212 - Role Strategy Plugin does not work with Hudson 3.3.0
Bug 474329 - After 3.2.2 migration to 3.3.0 CannotResolveClassException hudson.plugins.active_directory.ActiveDirectorySecurityRealm
Bug 475233 - Hudson places an X-UA-Compatible meta element at the BOF
Bug 476391 - Hudson Test Case failures due to multiple loading and multiple registration of plugin classes
Bug 477866 - Tool settings in the Hudson configuration page is not saved
Hudson 3.3.0.
Hudson 3.3.0 is a major release which upgrades the baseline Java requirement to run Hudson to at least Java 7. As part of this upgrade we have also refreshed some of our libraries, such as Apache Commons file upload. Because of these library changes, many plugins have been revised and the release uses a separate Plugin Center. Please report any issues at bugs.eclipse.org

Dependency and Library Changes
Bug 457805 - Upgrade Hudson baseline Java version to 7
Bug 459703 - Certify JDK 7 and 8, drop support for JDK 6 (and JDK 5)
Bug 445030 - Upgrade commons-fileupload to 1.3.1
Bug 445029 - Upgrade to Xalan 2.7.2
Bug 444314 - Upgrade to XStream v. 1.4.7 to fix Hudson not compatible with Azul Systems Open JDK
Bug Fixes
Bug 471476 - Lists of Build Executor Status and Build Queue disappear after a few seconds
Bug 471474 - When “More…” is clicked from the Build History area, all the existing builds will be displayed twice
Bug 471471 - Triggering a delayed build prevents any new builds for other jobs from running
Bug 460866 - SCM polling appears to hang Hudson
Bug 467144 - Dynamically created slaves don't belong to any team
Bug 434000 - WorkspaceCleanupThread removes workspace directories of the concurrent builds of a job before the builds complete
Bug 458304 - Hudson CLI login doesnt work with PAM Authentication
Bug 462865 - 'Tag this build' missing when using Team-based authentication
Bug 465870 - Create new Plugin Central for Hudson 3.3.0
Bug 465703 - Hudson 3.3.0 will require initial setup to be turned on
Bug 459811 - DNS Multicast should be disabled by default
Bug 386082 - Add the ability to disable a builder
Bug 414876 - Add the ability to add a description to each build step
Bug 444000 - Main hudson font looks bad on Chrome 37
Bug 445386 - Disable jmDNS multi-cast
Bug 446360 - Upgrade Bundled Jetty to v9.x
Bug 460095 - [MAC OS X] JDK 1.8 causes NoSuchMethodException: java.lang.UNIXProcess.destroyProcess(int)
Bug 463269 - Remove the Classic Plugin Manager from the Web UI
Bug 467293 - Job configuration not saved after upgrading to XStream 1.4.8
Hudson 3.2.2.
Hudson 3.2.2 is a patch-set release with no new features. Several of these bugs relate to security vulnerabilities and as such, we recommend the upgrade to all Hudson users. Please report any issues at bugs.eclipse.org

Bug Fixes
Bug 445412 - Icon color exception in hudson.log.
Bug 438894 - [Cascade] Continuous SCM polling when trigger configuration is done in cascading parent
Bug 445084 - Error in matrix filter result in HTTP error 500 and job cannot be saved anymore
Bug 447041 - Subversion plugin doesn't store credentials the way it should be.
Bug 457113 - [Cascade] Unnecessary calls of Trigger.run()
Bug 457650 - [Cascade] Overriding SCM Poll or Timer Trigger value of a Cascading child does not work without restarting Hudson
Bug 457654 - [Cascade] Newly created cascading job does not inherit triggers from parent without restart
Bug 422831 - cc.xml only knows "unknown" and "failure"
Bug 443880 - Hundreds of warning messages per minute being logged from hudson.util.RobustReflectionConverter
Bug 443944 - Renamed job leaves artifacts which prevents system from starting
Bug 452191 - Job should not be deleted when git polling is running, and vice-versa
Bug 452400 - Flaky ItemListener can leave job in half-deleted state
Bug 454261 - Performance issue with LOGGER.debug in SidACL
Bug 412421 - List of changes in a build has nonsense change-numbers (mercurial as SCM)
Bug 423108 - Remember Password does not work
Bug 445550 - Incomplete loading of Hudson dashboard page
Bug 447448 - Hudson JNLP Slave launching blocked with JDK 7_45 and above
Bug 453946 - All slaves are initially checked for Multi-configuration jobs
Bug 447469 - [Security] Disable SSL3 in Jetty Container to avoid poodle vulnerability
Bug 454550 - [Security] A deleted user can still run commands and is not removed from matrix security
Bug 454554 - [Security] Add header X-Frame-Options=sameorigin to prevent clickjacking vulnerability
Bug 454948 - [Security] Directory traversal vulnerability
Bug 454950 - [Security] XSS vulnerability
Bug 456244 - hudson/security/FederatedLoginService/UnclaimedIdentityException/error.jelly produces wrong message
Bug 458312 -[Security] XML External Entity Injection vulnerability
Hudson 3.2.1.
Hudson 3.2.1 contains a small number of bug fixes (listed below) and no new features. Please report any issues at bugs.eclipse.org

Bug Fixes
Bug 442020 - Unable to edit multijob configuration after update to 3.2.0
Bug 442067 - Hudson fails to start with Recursive load error after upgrade to Hudson 3.2.0
Bug 443742 - Promotion plugin not running promoted job after upgrading to 3.2.0
Hudson 3.2.0.
Hudson 3.2.0 is a major Hudson release which uplifts the Spring Framework version used in the Hudson core. This set of dependency changes has resulted in significant changes to the IP log and may cause problems with some, more obscure, plug-ins. Please report any issues at bugs.eclipse.org

New Features
Upgrade to Spring 3.1.2. As well as keeping us current, this change has allowed Hudson to adopt an up to date version of Spring security improving the overall secureness of the server.
Improvements to Team Concept. The team concept feature was introduced in 3.1.0 of Hudson. This release builds on our experience of using that feature in the real world and adds several enhancements and bug fixes in this area.
Note: If you use the capability within Team Concept to allocate a specific slave node to a team and you are running said node on Windows as a service, then you will need to remove and re-create the Windows Node if it was created by an earlier version of Hudson. This is because the launching command embedded into the service definition now includes the team information and this will be missing if the service was defined before the Team Concept feature was available.
Bug Fixes and Enhancement Details
Bug 441102 - [Team concept] Team matrix jobs get stuck waiting for executor
Bug 440682 - "Keep this build forever" and "delete this build" buttons do not display when using Teams and user is not sysadmin
Bug 440003 - Change Hudson Project Link on UI Pages to point to Eclipse
Bug 439801 - Public Nodes visible to teams are not enabled by default
Bug 439300 - Selecting "Subversion Workspace Version" 1.7 checks-out workspace as 1.8
Bug 439273 - Remove links to old Hudson wiki from UI help topics
Bug 439198 - Dead link in help topic for Use SSL in E-mail notification configuration
Bug 438400 - 1.8 Is not a permitted Subversion Workspace Version in manage hudson
Bug 438084 - [Team Concept] Master not utilized even when enabled for a team
Bug 437311 - [Team jobs] Public jobs should not be tied to any node other than master
Bug 437310 - [Team Concept] Team slave visible to teams that do not have permission to view/share slave in Job Configuration
Bug 437307 - [Team Concept] Clicking on script console for a team slave results in exception
Bug 437297 - [Team Concept] Team user with configure view/node permission is unable to configure view/node
Bug 437232 - [Team Concept]: Add ability to configure views per team
Bug 437224 - Link to download hudson-cli.jar downloads _hudson-cli.jar instead
Bug 437221 - Exception configuring security first time
Bug 437078 - [Team Concept] Node shared by another team must be explicitly approved by team Admin to build job from that team
Bug 436873 - [Team Concept] Master not visible to all teams
Bug 436868 - [Team Concept] Cannot delete team with jobs which have a large number of builds
Bug 436731 - [Team Concept] Manage Views/Nodes in team management page does not display anything when team admin manages multiple teams
Bug 436721 - [Team Concept] Cannot set default view for a team only the whole of Hudson
Bug 436713 - [Team Concept] User with node configure permission cannot mark the node offline
Bug 436712 - Hudson own user database sign up does not ignore white space from usernames
Bug 436684 - [Team Concept] Users who are system administrators should have a ui hint they are a super user
Bug 436654 - [Team Concept] Renaming a team view/node removes the view/node
Bug 436621 - [Team Concept] 404 when clicking on Master node
Bug 436620 - [Team Concept] Cannot launch slave created as a team node
Bug 436618 - [Team Concept] Team user/admin with configure node permission cannot configure/delete a slave
Bug 436518 - [Team Concept] Team slave is accessible using URL
Bug 436517 - [Team Concept] SysAdmin could mistakenly tie a team job to a node not visible to team
Bug 436507 - [Team Concept] Only sysadmin can create a job
Bug 436500 - [Team Concept] Private "My View" created by team member appears in the team's Views tab
Bug 436493 - [Team Concept] User with node create permission has no way to create a new node
Bug 436492 - [Team Concept] Remove the Debug Stack trace
Bug 436475 - [Team Cnocept] Cannot save matrix security or job based matrix security settings get a 500 error
Bug 436270 - [Performance] Hudson Initialization loads all the builds in jobs
Bug 436269 - [Performance] Switching/creating views with a large number of jobs is slow
Bug 436268 - [Performance] Logrotator loads all builds of jobs twice
Bug 435876 - Improve messages logging for slave nodes
Bug 435853 - Job page should not show a workspace link unless there is a workspace
Bug 434686 - 3.2.0: Server fails to start with existing Hudson home with security enabled
Bug 434515 - Plugin central should have an obsolete status so users can be advised during initial setup
Bug 434460 - 3.2.0: webapp not updated in hudson home when starting server with an older hudson home
Bug 434459 - 3.2.0: Server fails to start with hudson home from 3.1.2 using team authorization
Bug 434445 - Better presentation needed for sparse matrix build results
Bug 433930 - SPRING3: Upgraded Favorite plugin installs but the favorite filter does not work in a view
Bug 433488 - SCM Post-commit notification should not require authentication
Bug 433454 - role-strategy: Project roles are not working
Bug 433353 - Hudson Plugin Center does not show warning icon if a plugin was failed to load
Bug 433202 - Copy artifact fails to load.
Bug 432530 - RFC: Slave Ownership
Bug 431724 - Anonymous access to securityManager
Bug 431644 - [Spring 3.x] Creating a new job throws exceptions when logged in via ldap
Bug 430803 - While uploading a plugin dependencies of the uploaded plugins are not installed
Bug 430544 - "More" doesn't work
Bug 430537 - Test result extremely slow to display with large test
Bug 430314 - Hudson URL: Everything after host and port is ignored in a custom Hudson URL
Bug 430085 - [Cascading] Trigger build remotely is broken after upgrading to 3.1.2
Bug 429255 - Update maven-hpi-plugin to use Hudson 3.1.x for proper JDK 7 support
Bug 426705 - JOB_URL wrong if hudson runs behinds Apache
Bug 426681 - Hudson buildWithParameters rest api returns success for disabled job
Bug 426205 - Nested View plugin: Views can not be managed on team based security
Bug 422697 - runmap.xml not updated when job configuration is modified to not retain older builds
Bug 421939 - Link more in build history doesn't work
Bug 421233 - Upgrade support to Subversion 1.8 by using svnkit 1.8.0
Bug 419562 - threads deadlocked on hudson.model.Job.getCascadingProject(Job.java:1695)
Bug 416178 - New Plugin Center does not display warning about newer parent version
Bug 416025 - Team Concept: Add ability to configure slaves per team - and disable in job config
Bug 411044 - Team Concept: Moving jobs between teams does not refresh the Manage Team Jobs page
Bug 410363 - Git plugin push notification
Bug 405684 - Hudson security doesn't save jnlp port settings
Bug 390848 - Plugin under development is not listed in the new plugin manager
Bug 390382 - Upgrade spring-beans to a newer version (3.1.2)
Bug 368350 - Git holds open files on disk so they cannot be erased
Hudson 3.1.2.
Hudson 3.1.2 is a small bug-fix release on top of Hudson 3.1.1, and adds no new features or dependencies.

Bug Fix Details
Bug 424957 - Add external control for 3.1 cache purge interval
Bug 425948 - Often concurrent job building finishes with the error, the Workspace does not exist
Bug 426475 - Cannot create new maven 2/3 job
Bug 425187 - [Cascading] Polling Log page no longer works with cascading jobs (Hudson 3.1.1)
Bug 424012 - 3.1.1 LOADING overlay stays active on config page
Bug 425441 - [Cascading] Empty builders list written out to child jobs when restarting Hudson with cascading jobs
Bug 425459 - Saving job configuration fails when setting Maven 3 dependencies build property
Bug 425639 - Add job configuration view permission to Team permissions set
Bug 424967 - [Cascading] after upgrade from 2.2 to 3.1.1, unable to run jobs with parameterised cascading parent
Bug 425202 - [Cascading] Child jobs "forget" inherited configuration elements after update to 3.1.1
Hudson 3.1.1 Production.
Hudson 3.1.1 is a bug-fix release on top of Hudson 3.1.0, and adds no new features or dependencies.

Bug Fix Details
Bug 414867 - Add ability to copy teams
Bug 420625 - Exception with External job / Team management
Bug 414953 - [cascading] Renaming a cascading parent job does not update references in the children
Bug 420335 - Join plugin causes exception
Bug 407684 - Editing a multi-configuration job adds additional labels to configuration
Bug 418718 - REST: Creating job with REST by team member creates a team job without the team qualifier in the job name
Bug 420532 - Length of job name is not checked when renaming a job
Bug 420534 - CLI: Job name length is not validated
Bug 422279 - Cannot rename a public job
Bug 421272 - [Cascading] Allow parent to be deleted when all cascading children are deleted
Bug 401690 - Aborted and disabled projects are represented by the same gray circle
Bug 399069 - [Cascading] Problem with saving project changes with cascading feature
Bug 422613 - Live lock in hudson.security.SidACL.<init>(SidACL.java:36)
Bug 421671 - XML API picks team at random for non sys admin jobs
Bug 421672 - Team concept not supported in XML APIs
Bug 390895 - [Cascading] Support cascading of all or most job configuration options
Bug 390900 - [Cascading] Revert button not displayed on overridden JDK property
Bug 378810 - NullPointer exception when running matrix job with file parameter
Bug 396275 - Matrix jobs don't support concurrent execution
Bug 418652 - Error while starting client on HP-UX B 11.31 (ia64) OS: java.lang.UnsatisfiedLinkError: jnidispatch (/com/sun/jna/hp-ux-ia64n/libjnidispatch.so) not found in resource path at com.sun.jna.Native.loadNativeLibraryFromJar(Native.java:703)
Bug 390862 - [Cascading] Can't delete job copied from cascading parent
Bug 406889 - [Cascading] Non overridden job properties or properties with no values should not be written to config.xml
Bug 422041 - [Cascading] Inherited build parameter is reverted to original value after restart
Bug 422275 - [cascading] Performance problem with hasCyclicCascadingLink() and LDAP security
Bug 420832 - Promoted builds plugin does not show badges
Bug 418969 - <td> tag in description breaks page formatting
Bug 419775 - Hudson log no longer shows build completed status or build aborted messages
Bug 420426 - Too long job name throws exception
Bug 420686 - Team names should allow _ and - characters
Bug 410242 - [Cascading] Inherited Trigger settings don't cascade when changed, require Hudson restart
Bug 402679 - Test Result trend graph is completely blue, doesn't display failed tests
Bug 414463 - Multi-configuration child jobs tied to particular slaves do not show up in "Jobs tied to <slave>" list
Bug 418645 - Hudson disk-plugin is half-broken (does not display the size in job page) after upgrade to 3.1.0
Hudson 3.1.0.
Hudson 3.1.0 is a major release which introduces two key new features. This release is not yet production but is available for user testing. Please report any issues at bugs.eclipse.org

New Features
Team Concept. This new feature adds support for team based authentication and authorization model within Hudson. For a basic introduction head over to the Hudson Blog
Memory Management Changes. Hudson has received a major overhaul in the way that it manages the lists of Jobs and Builds that it maintains when it is running. This can lead to significant savings in heap usage from between 50% - 75% for a moderately complex build environment
Release Notes
Security Advisory

There is a possibility of cookie theft if a user uses the "Remember Me" functionality of Hudson when logging on if (and only if) Hudson is running in HTTP mode. In this situation it would be possible for a hacker on your local network to monitor the network traffic to your machine and extract the cookie that is used for the Remember Me feature. This cookie can then be used to impersonate that user until it expires. To avoid this risk we recommend that

You don't use the Remember Me function when logging on, (in any case, this function should only ever be used when access to the machine in question is secured and not shared) and / or
Run Hudson in HTTPS mode
For information on running Hudson standalone with Jetty in HTTPS mode see the Wiki Page on Starting and Accessing Hudson which details the command line parameters to enable HTTPS mode.
Bug Fixes and Enhancement Details
Bug 382490 - Installing JNLP Slave as windows service fails
Bug 383280 - Hudson should write a partial build.xml so jobs interrupted by a restart are visible.
Bug 402338 - /api/schema URL on Hudson returns NoClassDefFoundError
Bug 404822 - Upgrade guava to 12.0 or 13.0.1
Bug 412720 - Team Concept: If a user is admin in two or more teams he only can manage one team in his manage team view
Bug 384554 - Hudson with many jobs takes very long time to start
Bug 375475 - Group administration user interface
Bug 376917 - Maven 3.0.4 is not supported
Bug 387431 - Per job rename/delete permission
Bug 392254 - Allow jobs to be grouped hierarchically
Bug 400722 - LDAP group query cannot be overridden
Bug 400981 - JNA Native Support Plugin breaks slave jobs
Bug 404101 - Bundled releases should behave like ordinary releases by default
Bug 405680 - Hudson slaves execute native commands via Ant instead of JNA
Bug 406601 - Add support for Multi-tenancy/Team Concept in Hudson
Bug 407132 - Team Concept: Build job names should be unique within a Team only.
Bug 407133 - Team Concept: While migrating handle all existing job as Global jobs and read only
Bug 409881 - When Hudson run in JDK 7 plugins won't build in it
Bug 411537 - 3.0.1 parent still uses maven-hpi-plugin:3.0.1
Bug 412722 - Team Concept: Jobs dont show up in view for sysadmin
Bug 413554 - Team Concept: Sidebar in Job page is not updated when Build Now clicked
Bug 414176 - Performance: Hudson fails to load Legacy maven jobs with sub modules
Bug 366134 - Redirect Embedded Security WIKI link in help to Eclipse
Bug 411710 - Team Concept: Deleting a team with jobs does not make the jobs public
Bug 413085 - Team Concept: Jobs can be moved into a team more than once
Bug 413367 - Team Concept: Provide a way to find the teams that a member belongs to
Bug 414603 - Team Concept: CLI: Copy job to a new team fails with NPE
Bug 414846 - Can't delete job
Bug 415001 - Build Name and Description Not Shown in Build History
Bug 415087 - Team Concept: Mixed case user names do not work correctly with teams
Bug 415103 - create-job CLI command with no parameters throws NPE
Bug 415173 - Team Concept/CLI: Team name is not validated when creating team using CLI command create-team
Bug 415242 - Team Concept: CLI: create-job throws NPE
Bug 415270 - Team Concept: CLI: create-job fails when trying to create a job in a team when the job name matches the name of a public job
Bug 415357 - Deleting team with a job whose unqualified name is same as public job throws IllegalArgumentException
Bug 415386 - CLI: update-job throws exception when run without any parameters
Bug 415387 - Team Concept: CLI: create-job/copy-job should not allow period names
Bug 407926 - Typo on Manage Hudson page, "Configure Authentication and Authorization *Stretegy* ..."
Bug 409430 - Team: Cannot add jobs to a view/cannot view job details for jobs in a view
Bug 410381 - Team Concept: Clicking on Move Jobs button fails in IE 8 & IE 9
Bug 410382 - Team Concept: Move job fails on Windows
Bug 410383 - Team Concept: Error message cannot be viewed without scrolling when move jobs fails
Bug 410540 - Team Concept: LDAP roles/groups: LDAP user who is part of a role which is added to a team is unable to see team jobs
Bug 410631 - Team Concept: Renaming a job associated with a team makes the job visible only to sysadmin user
Bug 410845 - Migrating server from 3.0.1 to 3.1.0 fails with NPE
Bug 410851 - Team Concept: Shared job: Cannot copy from team job/cannot copy from a shared job
Bug 410854 - Team Concept: Manage teams UI on Windows 7 is incorrect
Bug 410855 - Team Concept: Team job details are not accessible when switching from team authorization to no authorization
Bug 411044 - Team Concept: Moving jobs between teams does not refresh the Manage Team Jobs page
Bug 411707 - Team Concept: Mange teams page UI design changes
Bug 412237 - Team Concept: Manage Teams: Job status icon is not displayed for team jobs
Bug 412242 - Team Concept: Error message when adding a team name cannot be viewed without scrolling
Bug 412243 - Team Concept: New team: Previous details including error message are displayed
Bug 412695 - Team Concept: Cannot build job from command line
Bug 412696 - Team Concept: Hudson local database: Cannot login from the command line
Bug 413015 - Team Concept: Add New Team dialog is not scaled correctly and has scrollbars
Bug 413184 - Team Concept: Server fails to start with NPE after moving a parent job to a new team
Bug 413185 - Team Concept: Team jobs are public by default
Bug 413274 - Team Concept: Member of multiple teams: Job permissions can conflict
Hudson 3.0.1 Production.
Hudson 3.0.1 is mainly a bug-fix release on top of Hudson 3.0.0, as well as the changes here in Hudson Core, many plug-ins have been fixed up to work correctly with Hudson 3.0.

New Features
New Core property: Privacy Message. This optional text will be printed in the footer of every page of the Web UI. It is intended as a place where admins can advertise the relevant privacy, confidentiality or copyright statements that may be reqired for the site. This String value can contain markup for formatting and links if required. The value is configured on the main configuration screen
New Core property: Instance Tag. This optional text will be printed in the header, footer and window title of every page of the Web UI. It allows admins who have several Hudson instances to manage to identify which instance they are working with at a glance, e.g. "Production", "Sandbox" etc. The tag used is an arbitrary String value and it is recommended that you keep the value short so that it can be fully displayed in the available space. Markup is ignored in this property. The value is configured on the main configuration screen
Green balls are now the default for a successful build rather than blue. You can revert to using a blue ball via an option in the main configuration screen (if you really want to!). Red balls are still red.
New Hudson Branding Bar. The footer of each page carries a branding bar containing the text Powered by Hudson Open Source Continuous Integration Server from the Eclipse Foundation
Bug Fixes and Enhancement Details
Bug 388134 - HudsonTestCase#recipeLoadCurrentPlugin() fails to load current plugin
Bug 389581 - Could not compile custom Hudson plugin in Java 1.7
Bug 399342 - Using ${project.parent.version} in hudson-plugin-parent makes multi-level poms misbehave
Bug 397964 - Highlighted item on Job Copy drop down list has poor color combination
Bug 401754 - Need a way to identify Hudson Instance on all releases
Bug 401827 - Mentions of old java.net email lists should be updated to Eclipse dev list
Bug 401901 - Help for Hudson Search function needs to be pointed to Eclipse WIKI
Bug 402138 - Python API references need to be removed from /api screen
Bug 402428 - Changes to implement instance labeling and privacy message break translation plugin
Bug 398310 - HUDSON_BUILDS variable makes a mess
Bug 388589 - Cannot determine Hudson-Version from project dependencies when building 3.0.0 Plugin
Bug 393709 - System configuration never loads
Bug 395879 - thread deadlock
Bug 398313 - Launching Hudson with --daemon doesn't put it in background anymore
Bug 398315 - init.groovy doesn't work anymore
Bug 399218 - Use a green ball for successful test runs instead of a blue ball
Bug 399405 - Test Result Trend Graph doesn't display Failed tests
Bug 399631 - Passwordless Emails Fail
Bug 401134 - Fix the confusion between project and job nomenclature in Hudson
Bug 401683 - After renaming node references from all jobs are lost to this node
Bug 402965 - Markup Formatters do not load in Hudson 3.0
Bug 403703 - HudsonTestCase breaks subclasses in plugins that depend on other plugins
Bug 403732 - Job Health is not correctly reported in 3.0.1 candidate
Hudson 3.0.0 Production
New Features / Changes
Unlike the release candidates administrators have the ability to install and use Hudson without having to initially load pre-requisite plugins. These plugins are now marked as recommended but are not required. We do, however, strongly recommend that you install the suggested plugins in order to achieve the full value from your Hudson server.
Hudson 3.0.0 Release Candidate
New Features / Changes
Minor UI Tweaks to improve the Eclipse Branding match
Further changes to the update manager and initial install setup UIs in response to community feedback
Specific Issues Addressed
Bug 369065 - All Automatic JDK installation options are disabled
Bug 371199 - jUnit graph no longer link to testing result.
Bug 371200 - Background grid on the chart on the load statistics page doesn't scale to short and medium well
Bug 371625 - Fine logging shows job success issue
Bug 382490 - Installing JNLP Slave as windows service fails (Risky fix, so disabled the feature for now)
Bug 382686 - Configure link shows up for a job even when user does not have privileges to configure the job.
Bug 382033 - On windows sometimes a plugin will error downloading during Initial Setup
Bug 388542 - Remove unwanted library jaxb-impl-2.2.5.jar from core distribution
Bug 388295 - Delete job with no builds in 3.0.0-RC2 causes hudson to stall
Bug 392423 - Icon paths do not add the prefix when resolved so missing icons
Bug 392580 - 404 error deleting a job if you are using --prefix argument
Hudson 3.0.0 Milestone 4
New Features / Changes
Finalization of all shipped libraries in line with those approved through the Eclipse IP Process
Updated plug-in Manager pages providing an improved management experience for your plug-ins
Hudson 3.0.0 Milestone 3
New Features / Changes
More IP and Library Cleanup
New Initial setup screen to help you to configure the instance and it's plugins on first run. You can read more in the WIKI
Provision to provide alternate update site via command line. You can now pass the ----updateServer=<your Update Server> parameter as a command line argument to configure a custom update center
Hudson Security Setup is separated out of Global configuration page in to its own configuration page accessed via a link in the management page
Known Issues
The M3 release has some issues which are not show-stoppers but which you should be aware of:

BugZilla 382522: PAM security will not allow users to authenticate after upgrade
BugZilla 382490: Installing JNLP Slave as windows service fails
BugZilla 382033: Windows configuration issues with Initial Setup
BugZilla 382500: Testing PAM authentication fails
Hudson 3.0.0 Milestone 2
This Eclipse release has been concentrating on further library clean up, both in terms of the number of libraries used and the versions...

Externalize Groovy into a separate plugin for IP reasons
Rewrite of Hudson security to remove Groovy dependency
Hudson fork of Stapler and Jelly now used by Core
jsr305-1.3.9.jar dependency removed
joda-time.1.5.1.jar dependency removed
jcaptcha plugin removed from Core distro
jfreechart plugin removed from Core distro and replaced with BIRT
Hudson 3.0.0 Milestone 1
This Eclipse release has been concentrating on further library clean up, both in terms of the number of libraries used and the versions...

access-modifer-annotation-1.0.jar dependency removed
bridge-annotation-method-1.4.jar dependency removed
crypto-util-1.0.jar dependency removed
embedded_su4j-1.1,jar dependency removed
logkit-2.0.jar dependency removed
Stapler downgraded to version 1.155 and forked into the Eclipse codebase
Stapler-groovy downgraded to version 1.155 and forked into the Eclipse codebase
Stapler-jelly downgraded to version 1.155 and forked into the Eclipse codebase
Hudson 3.0.0 Milestone 0
This Eclipse release has been a huge task with a large amount of re-factoring, IP work and library upgrades. Here's a summary..

Remove all the dependency on GPL/LGPL libraries
Apply for IP approval for individual components of Hudson code base
Apply IP approval for external JavaScript libraries bundled with Hudson
Use latest Original libraries rather than the original "Hudson special" patched versions
New Eclipse look and feel for the branding
Replacing the use of LGPL licensed JFreechart with Eclipse BIRT charts
Replacing the original Winstone server with Jetty
Check-in all Hudson code base to Eclipse git
Start continuous builds at Eclipse Hudson
Stabilize the code base after several library and technology changes arise due to IP review process
QA certify the product before M0 release
Manage the IP process for external Java libraries bundled with Hudson (Some are still pending)
Manage the IP process for build and test libraries used to build and test Hudson (Pending)
All the work on the website and the Eclipse wiki
Furture changelog entries will take a more conventional - here are the bugs we've fixed approach, but we just thought that you'd like to see what work goes on behind the scenes of a release like this

Earlier Releases
The changelog for earlier (non-Eclipse) versions of Hudson can be found here.

Fonte: https://eclipse.org   ;  https://eclipse.org/hudson/changelog.php



