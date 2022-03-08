# 记账软件

1.contentView

```swift
import SwiftUI

struct ContentView: View {
    
    @ObservedObject  var expenses = Expenses()
    @State private  var showSheet:Bool = false
    
    var body: some View {
        NavigationView{
            List{
                ForEach(expenses.allExpenses){expens in
                    HStack{
                        VStack(alignment: .leading){
                            Text(expens.name).font(.headline)
                        Text(expens.type)
                    }
                        Spacer()
                        Text("\(expens.amount)")
                    }
                    
                }
                .onDelete(perform: remove)
                }
            .navigationTitle("费用")
            .navigationBarItems(trailing: Button(action: {
                showSheet = true
            }, label: {
                    Image(systemName:"plus")
               }
                )
            ).sheet(isPresented: $showSheet) {
                addView(expenses: expenses)
            }

                
            }
            
            
            }
    func remove(indexSet: IndexSet){
        expenses.allExpenses.remove(atOffsets: indexSet)
    }
//            List(expenses.allExpenses){ expens in
//                HStack{
//                    VStack(alignment: .leading){
//                        Text(expens.name).font(.headline)
//                    Text(expens.type)
//                }
//                    Spacer()
//                    Text("\(expens.amount)")
//                }
//
//            }
            
            
        }

    
    
    
    


struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

```



2. addView

    ```swift
    import SwiftUI
    
    struct addView: View {
        static let types = ["生活日用","数码加电","学习"]
        @State private var type = 0
        @State private var name:String = ""
        @State private var amount = ""
        @ObservedObject  var expenses : Expenses
        
        @Environment(\.presentationMode) var presentationMode
        
        var body: some View {
            NavigationView{
                Form{
                    TextField("支出名",text: $name)
                    Picker("种类",selection: $type){
                        ForEach(Self.types.indices,id:\.self){
                            Text(Self.types[$0])
                        }
                    }
                    TextField("花费",text: $amount)
                        .keyboardType(.numberPad)
                }
                .navigationBarTitle("添加支出")
                .navigationBarItems(trailing: Button("保存"){
                    expenses.allExpenses.append(Expense(name: self.name, type: Self.types[self.type], amount: Int(self.amount)!))
               
                    
                    
                    presentationMode.wrappedValue.dismiss()
                })
            }
        }
    }
    
    struct addView_Previews: PreviewProvider {
        static var previews: some View {
            addView(expenses: Expenses())
        }
    }
    
    ```

3. Expense

    ```swift
    import Foundation
    
    struct Expense:Identifiable,Codable {
        let id = UUID()
        let name: String
        let type: String
        let amount: Int
    }
    
    
    class Expenses:ObservableObject{
        @Published var  allExpenses : [Expense]{
            didSet{
                if let data = try?
                JSONEncoder().encode(allExpenses){
                    UserDefaults.standard.set(data, forKey: "allexpenses")
                }
            }
        }
        
        init(){
            if let data = UserDefaults.standard.data(forKey: "allexpenses"),
               let allExpenses = try? JSONDecoder().decode([Expense].self,from: data){
                self.allExpenses =  allExpenses
                return
        }
            allExpenses = [
                Expense(name: "Macbook", type: "数码加电", amount: 150000),
                Expense(name: "IOS开发课程", type: "学习", amount: 1999)
            ]
        
    }
    }
    ```

    