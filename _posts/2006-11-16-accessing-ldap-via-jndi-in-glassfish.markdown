---
layout: post
title: Accessing LDAP via JNDI in Glassfish
---

STEP 1: Create the JNDI LDAP Resource

In the GlassFish admin console create a JNDI Custom Resource with the following parameters:

1. Give it a JNDI name such as myLDAP
2. Resource Type: javax.naming.directory.Directory
3. Factory Class: com.sun.jndi.ldap.LdapCtxFactory

Add the following as additional parameters:

1. Name: java.naming.security.principal Value: the reader dn
2. Name:java.naming.security.credentials Value: the password
3. Name: URL Value: ldap://servername/baseDN

STEP 2: JAVA code to access the LDAP

Use code similar to the following to access the LDAP:

		#!java
		try {
			Context initCtx = new InitialContext();
			DirContext ctx = (DirContext) initCtx.lookup("myLDAP");

			SearchControls ctls = new SearchControls();
			ctls.setSearchScope(SearchControls.SUBTREE_SCOPE);

			String searchfilter = "(mail="+ email +")";
			NamingEnumeration answer = ctx.search("", searchfilter, ctls);

			if(answer.hasMore()) {
				SearchResult entry = (SearchResult) answer.next();
				Attributes attrs = entry.getAttributes(); 
				......
			} else {
				success = false;
			}
		} catch (NamingException e) {
			e.printStackTrace();
		}