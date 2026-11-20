Genel sektör için bir güvenlik protokolüdür. Genel cihazlr ve ağlar için yerel güvenlik akışları oluşturmaktadır. 

#### Nasıl Çalışır 
1. **Uygulama**, kullanıcıyı yetkilendirme sunucusuna yönlendirir.
    
2. **Kullanıcı**, giriş yapar ve uygulamanın hangi bilgilere erişeceğini onaylar.
    
3. **Sunucu**, uygulamaya bir **authorization code** verir.
    
4. **Uygulama**, bu kodu kullanarak **access token** alır.
    
5. **Uygulama**, bu token ile API'ye istek atar ve veriye erişir.

Oauth bir kimlik doğrulama protokolünden ziyade bir yetkilendirme protokolüdür. 
Genel doğrulamakar içn arka planda OpenID teknolojileri çalışmaktadır. 


Protokol kurulumunda Oauth geliştiricileri genel olarak bir diyagram yayınlamıştır.
![[Pasted image 20261008093018.png]]


Bu ksısımda 2 ayrı protokol kurabiliriz. 

#### 1. Sunucu Güvenliği 
- Bu kısımda kurduğumuz protokol yığını ayrı bir sunucuda tutulur. 
- Uygulama siz ve sunucu arasında bir onay durumu başlatır. 
- API'niz sadece tokenı doğrular tokeni üretmez. 
- Birden fazla API aynı auth sunucusunu kullanabilir. 

##### API içine Gömülü Protokoller 

- API hem token üretir hem doğrular. 
- Oauth açısından önerilmez ama bazen pratik olabilir. 


|Çözüm|Tip|Uygun Olduğu Yer|
|---|---|---|
|**Keycloak**|Açık kaynak, self-hosted|Kendi sunucunuzda tam kontrol|
|**Auth0**|SaaS|Hızlı başlangıç, ücretli|
|**Okta**|SaaS|Kurumsal|
|**Azure AD / Entra ID**|SaaS|Microsoft ekosistemi|
|**Google Identity**|SaaS|Google ekosistemi|
|**AWS Cognito**|SaaS|AWS projeleri|
|**Duende IdentityServer**|Kütüphane (.NET)|Kendi kodunuza gömer|
|**Ory Hydra / Keycloak**|Açık kaynak|Self-hosted alternatif|

Pythonda AuthLib javascript ve node üzerinde oidc ve provider ile lendi servislerinizi yazabilirsiniz. 

JWT bir token doğrulama servisidir. AWS ve Auth 2.0 gibi servislerin alt katmanlarında çalışır 
- OAuth: "Kullanıcı login olur → onay verir → code alınır → token alınır" gibi **adımları** tanımlar.
    
- JWT: `header.payload.signature` şeklinde **tek bir string**dir. İçinde ne olduğu format olarak tanımlıdır.

Projelerinizde AOuth 2 kullanmak için piyasada kullanılan hazır bir yetkilendirme sunucusu en doğru yaklaşımdır. 
Bu sunucu token üretme,kullanıcı doğrulama ve izin yönetimi gibi karmaşık işleri halletmektedir. 

Piyasada bu protokole uygun yazılımlar bulunmaktadır. 

- KeyCloak : Açık kaynaklı ve kendi sunucunuzda barındırabilirsiniz. 
- Duende ıdentityServer : .Net ekosistemi için standarttır. 
- dotnet new şablonlarını kullanmaktadır. 
- Spring Auth : Java/Spring boot projeleri için resmi ve modern çözümdür. 
- Auth0 OKta ve Azure 3D : Bulut Tabanlı SaaS çözümleridir. Altyapı yönetimiyle uğraştırmaz. 


Eğer özel bir ihtiyacınız varsa ve kendiniz geliştirmek isterseniz OAuth 2.0 mantığını sıfırdan yazmak yerine bir kütüphane kullanmalısınız. 

Bu kısımda basic-oauth2-server veya Duende gibi kütüphanelerle kendi servisinizi inşa edebilirsiniz.
