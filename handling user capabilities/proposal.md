# Lions and tigers and handling user capabilities

Many applications restrict access to some features for some users for various reasons. For example: Only premium users get access to extra features. Only supervisors can edit product categories. When you are running A/B experiments, you want some users to have a feature and not others.

When rendering the templates, only some users should see the “Edit” button, or the “Daily Reports” tab. Obviously, your backend should handle these restrictions too, but if you are building your frontend code in JS, your frontend needs to know what restrictions your users have as well. How should you represent these restrictions, and can you centralize the logic? Where in the application should you check for and enforce them?

I went on a hunt to gather patterns and techniques for handling the logic around user capabilities in client-side apps. Join me on a safari through the approaches, and I’ll tell you what I have learned.

# Outline

- Times when you need to restrict access. Claim that many apps have some kind of user access restrictions. i.e. it will be something that comes up in your career.
- Naive ideas to implement: Are these approaches good / bad / where should the logic go?
- Seeking pre-existing approaches.
- There is a vocabulary for this in Computer Science (subject, object, operations (?) …, Principle of Least Privilege) and names for different approaches. Two most widely used / discussed: ACL, RBAC.
- Distinction / lack of distinction between ACL and RBAC. Demonstrate why ACLs effort to administrate is flawed (too much effort leads to granting too much access).
- Terminology of RBAC: users, roles, operations (capabilities), [sessions]
- Examples of operations a user with a role might conduct.
- (I have begun using the term operator in our user management system to distinguish from the users of the product.)
- Defining roles together with the people who perform the job, and assigning roles by / together with the people who manage / supervise the operators.
- What parts of the logic to put where? Model, view, controller? --> template

can this && that && theOther

ends up always being an or: can seeBillingAddress
get "/users/:user_id/billing_address"
	can(["seeBillingAddress", "editBillingAddress"])
end

Meta Lesson: If you know the domain, it is easy (/ obvious how) to write the code. What is hard is recognising when to step back, and it is hard to know what questions to ask.

# Many if not most web applications have access restrictions
[Times when you need to restrict access. Claim that many apps have some kind of user access restrictions. i.e. it will be something that comes up in your career.]
Not talking about permissions nor talking about “my things vs not my things”. Talking about

# THOUGHTS

## Seeking pre-existing approaches

When I first starting planning to add access restrictions to our user management system, I got worried that it was going to get messy and complicated. I assumed there must be pre-existing approaches that were recommended or discouraged.

## Defining roles
the designing here is uncovering the roles that make sense, organisationally. like any other aspect of the design, these may evolve over time and should therefore be change

## Translating capabilities to UI elements: interaction points

## RBAC vs ACL
John Barkley, Comparing Simple Role Based Access Control Models and Access Control Lists, 1997
“RBAC has several advantages over ACL. Even a very simple RBAC model affords an administrator the opportunity to express an access control policy in terms of the way that the organization is viewed, i.e., in terms of the roles that individuals play within the organization.”

**HTTP REST API endpoints as capabilities**: good way of thinking or not? too granular, but eventually each endpoint should be covered by a capability (some go together in one capability, such as POST/PUT/DELETE, which can be lumped together as *write*). The task of grouping capabilities should be done while thinking about “activities” or processes.

Below is sort of an aside / our justification for having restrictions.
**We restrict access to certain features not because of trust issues, but because our processes require training**. We do not deny the people who  do customer support write access to the database because we inherently trust them less then the developers, but because unless they have training on SQL, they could unintentionally cause damage. Similarly, we deny most of the developers access to some features of our administration system not because we think they will maliciously abuse that access, but because they have not been trained in our customer support policies. Also, sadly, as we grow as a company, we can’t rely on trust since we are more likely to actually trust the wrong person ...

# References

http://en.m.wikipedia.org/wiki/Access_control_list