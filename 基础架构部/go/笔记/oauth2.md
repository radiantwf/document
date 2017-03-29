# oauth2学习笔记

#### 安装
命令号运行：
 
    go get -v -u golang.org/x/oauth2
#### 引入
    import (
        "golang.org/x/oauth2"
    )
#### Example
##### password:
	ctx := context.Background()
	conf := &oauth2.Config{
		ClientID:     "O3gMpRJtROqw5v9WrYqm9zQlGAUjhFkojlNHFN7V",
		ClientSecret: "57ANxb2GRY3DZ3eQE7u6f1QN7TZKxCtaBWavy5ZPKTZokUjOlqjkIi3w6ZAL0yLd1ajdlhAYLYcfPitYFobLSghtDOIYIhJVlNxACumMyJfObEo7AymQvClM6pQQpIQC",
		Endpoint:     Endpoint,
	}
	tok, err := conf.PasswordCredentialsToken(ctx, username, password)
##### code:
    ctx := context.Background()
    conf := &oauth2.Config{
        ClientID:     "YOUR_CLIENT_ID",
        ClientSecret: "YOUR_CLIENT_SECRET",
        Scopes:       []string{"SCOPE1", "SCOPE2"},
        Endpoint: oauth2.Endpoint{
            AuthURL:  "https://provider.com/o/oauth2/auth",
            TokenURL: "https://provider.com/o/oauth2/token",
        },
    }
 
    // Redirect user to consent page to ask for permission
    // for the scopes specified above.
    url := conf.AuthCodeURL("state", oauth2.AccessTypeOffline)
    fmt.Printf("Visit the URL for the auth dialog: %v", url)
 
    // Use the authorization code that is pushed to the redirect
    // URL. Exchange will do the handshake to retrieve the
    // initial access token. The HTTP Client returned by
    // conf.Client will refresh the token as necessary.
    var code string
    if _, err := fmt.Scan(&code); err != nil {
        log.Fatal(err)
    }
    tok, err := conf.Exchange(ctx, code)
    if err != nil {
        log.Fatal(err)
    }
 
    client := conf.Client(ctx, tok)
    client.Get("...")