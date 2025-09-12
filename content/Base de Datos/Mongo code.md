###### compass #######
mongodb+srv://crhacker7:<password>@violetamongo.gvqrwm1.mongodb.net/

nLOvpbEBlWdY8W7e mongopassw

###### drive  ####
mongodb+srv://crhacker7:<password>@violetamongo.gvqrwm1.mongodb.net/?retryWrites=true&w=majority&appName=VioletaMongo

#### shell ######
mongosh "mongodb+srv://violetamongo.gvqrwm1.mongodb.net/" --apiVersion 1 --username crhacker7

#### si el puerto de mongo está en escucha ######
netstat -tlnp

#### para si tienes instalado el servidor de MongoDB ###########
ps aux | grep mongod
########################  MONGO ###########################

import com.mongodb.ConnectionString;
import com.mongodb.MongoClientSettings;
import com.mongodb.MongoException;
import com.mongodb.ServerApi;
import com.mongodb.ServerApiVersion;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;

public class MongoClientConnectionExample {
    public static void main(String[] args) {
        String connectionString = "mongodb+srv://crhacker7:<password>@violetamongo.gvqrwm1.mongodb.net/?retryWrites=true&w=majority&appName=VioletaMongo";

        ServerApi serverApi = ServerApi.builder()
                .version(ServerApiVersion.V1)
                .build();

        MongoClientSettings settings = MongoClientSettings.builder()
                .applyConnectionString(new ConnectionString(connectionString))
                .serverApi(serverApi)
                .build();

        // Create a new client and connect to the server
        try (MongoClient mongoClient = MongoClients.create(settings)) {
            try {
                // Send a ping to confirm a successful connection
                MongoDatabase database = mongoClient.getDatabase("admin");
                database.runCommand(new Document("ping", 1));
                System.out.println("Pinged your deployment. You successfully connected to MongoDB!");
            } catch (MongoException e) {
                e.printStackTrace();
            }
        }
    }
}

