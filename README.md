# Rust TCP Server. 

A simple and lightweight crate for launching and using a server. Server has Many or One Thread Mode. Not used libraries!
Documented [server_tcp.rs](https://github.com/Amakesasha/Rust-TcpServer/blob/main/src/server_tcp.rs) and [lib.rs](https://github.com/Amakesasha/Rust-TcpServer/blob/main/src/lib.rs)
Source [code link](https://github.com/Amakesasha/Rust-TcpServer)

# Usage example: 
 ``` Rust
extern crate rust_tcp_sever;
pub use rust_tcp_sever::*;

fn main() {
    Server::launch(TcpServer::new(
        TcpListener::bind("127.0.0.1:8080").unwrap(),
        ThreadPool::new(4),
    ));
}

struct Server;

impl ServerControl for Server {
    const TYPE_HTTP: Option<&'static str> = Some("HTTP/2.0");

    fn match_get(request: &Request, response: &mut Response) {
        match request.metod_url_http[1].as_str() {
            "/qwe" => {
                response.set_response("200 OK", "All Good)");
                response.response_add_content("Content-Type", "text/html");
                response.cookie.add("testName", "testValue");
                response.cookie.add("asdf", "fdsa");
                response.cookie.delete("asdf");
            }
            "/wer" => response.set_redirect("/qwe"),
            _ => {}
        }
    }
    fn match_post(_request: &Request, response: &mut Response) {
        response.set_redirect("/qwe");
    }
    fn match_put(_request: &Request, _response: &mut Response) {}
}
 ```