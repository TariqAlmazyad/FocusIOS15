# FocusIOS15 
## Super simple demo with minimal code in how to use @FocusState to optomize UI/UX for users. 


<img width="1784" alt="Screen Shot 2021-07-12 at 1 17 19 PM" src="https://user-images.githubusercontent.com/34104180/125271927-eb824900-e313-11eb-947b-19857d0c09d0.png">

```swift
//
//  ContentView.swift
//  Shared
//
//  Created by Tariq Almazyad on 7/12/21.
//

import SwiftUI

enum Field: Hashable{
    case emailAddress
    case fullName
    case username
}

struct ContentView: View {
    @FocusState private var focusState: Field?
    @State private (set) var emailAddress: String = ""
    @State private (set) var fullName    : String = ""
    @State private (set) var username    : String = ""
    var body: some View {
        NavigationView {
            VStack{
                Group{
                    TextField("email",     text: $emailAddress)
                        .focused($focusState, equals: .emailAddress)
                        .submitLabel(.next)
                    
                    TextField("full name", text: $fullName)
                        .focused($focusState, equals: .fullName)
                        .submitLabel(.next)
                    
                    TextField("username",  text: $username)
                        .focused($focusState, equals: .username)
                        .submitLabel(.done)
                    
                }.textFieldStyle(.roundedBorder)
                    .padding()
                Spacer()
            }
            .toolbar{
                ToolbarItem(placement: .keyboard) {
                    HStack{
                        // the reverse of the bottom
                        Button {
                            switch focusState {
                            case .emailAddress:
                                focusState = .fullName
                            case .fullName:
                                focusState = .username
                            case .username:
                                print("")
                            case .none:
                                print("")
                            }
                        } label: {
                            Image(systemName: "chevron.down")
                                .foregroundColor(focusState == .username ? .gray : .blue)
                        }.disabled(focusState == .username)
                        // the reverse of the top
                        Button {
                            switch focusState {
                            case .emailAddress:
                                print("")
                            case .fullName:
                                focusState = .emailAddress
                            case .username:
                                focusState = .fullName
                            case .none:
                                print("")
                            }
                            
                        } label: {
                            Image(systemName: "chevron.up")
                                .foregroundColor(focusState == .emailAddress ? .gray : .blue)
                        }.disabled(focusState == .emailAddress)
                        Spacer()
                        Button {
                            focusState = nil
                        } label: {
                            Image(systemName: "keyboard.chevron.compact.down")
                                .foregroundColor(.blue)
                        }
                    }
                    
                }
            }
            .navigationBarTitle("@FocusState", displayMode: .large)
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
            .preferredColorScheme(.dark)
    }
}

```




