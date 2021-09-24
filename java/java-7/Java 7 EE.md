Build in Scopes

- Application Scope: @ApplicationScoped
- Session Scope: @SessionScoped
- Request Scope: @RequestScoped
- Conversation Scope: @ConversationScoped
- @Dependant. All beans that explicitly doesnt specify are @Dependent. They will assigned the same scope as the beans they are injected into. So if a @Dependent bean is injected within a session scoped bean, it would get a session scope.

Conversation and Session are serializable and implements Serializable.