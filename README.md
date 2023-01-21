# Swift_Retry_Request

```swift
func retry<T: Decodable>(
    retries: Int,
    task: @escaping (_ completion: @escaping(Result<T, Error>) -> Void) -> Void,
    completion: @escaping (Result<T, Error>) -> Void
) {
    task { result in
        switch result {
        case .success:
            completion(result)
        case .failure:
            #if DEBUG
            print("\(result) retries left")
            #endif
            if retries > 1 {
                retry(retries: retries - 1, task: task, completion: completion)
            } else {
                completion(result)
            }
        }
    }
}

retry(retries: 3, task: fetchUser(completion: )) { result in
    switch result {
    case .success(let user):
        print(user)
    case .failure(let error):
        print(error.localizedDescription)
    }
}

```
