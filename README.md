🌿 BIO ORACLE: The Earth's Digital Memory

    "Data without sentiment is just a frivolity of evolution. We are here to anchor the soul of the biosphere into the eternity of the block."

👁️ The Vision

In a world obsessed with ephemeral transactions, BIO ORACLE emerges as a "Hélix Scanner" for the real world. We believe that every plant, every mycelium, and every ancient tree is a guardian of memory.

This project isn't just an app; it's a Decentralized Biological Oracle. We are building the first world-scale, immutable herbarium where the discovery of a child in a forest has the same historical weight as a laboratory's finding. Because if a species disappears and no one registered its "digital heartbeat," did it ever truly exist in our collective memory?
🧬 What is BIO ORACLE?

BIO ORACLE is a mobile-native platform (Android) that transforms the smartphone into a bio-validation node.

    The Capture: Using a high-fidelity interface (inspired by the Animus/Abstergo aesthetics), users scan botanical life forms.

    The Intelligence: Real-time AI identification decodes the biological identity of the specimen.

    The Anchor: Using SQLite for offline resilience.


    🛠️ The Bio-Stack

    Core: Kotlin & Jetpack Compose (The Interface of the Future).

    Persistence: SQLite (The Black Box: ensuring data survives even in the deepest forest).

    Identity: AI-Powered Botanical Recognition API.
🌍 Why it Matters (The Sentiment)

We are currently living through a "silent amnesia." Species vanish before they are even named.

    For the Community: It’s a game of "Battle Fog" where every scan lights up a dark map of our planet's health.

    For the Future: It's a legacy. A hundred years from now, a child will be able to see exactly what we breathed and what we protected, verified by the math of the blockchain.

BIO ORACLE is our "Salto de Fe." It is the technology bowing down to the roots of a fern. It is the realization that the most advanced hardware on this planet is not a chip, but a leaf.


SOME CODE OF API, WONKING!

```
import okhttp3.*
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.RequestBody.Companion.asRequestBody
import okhttp3.RequestBody.Companion.toRequestBody
import java.io.File
import java.io.IOException

/**
 * Cliente para la API de identificación de plantas de Pl@ntNet.
 * Documentación: https://my.plantnet.org/doc/identification
 *
 * @param apiKey Tu clave privada de la API (obtenida en my.plantnet.org)
 * @param project Proyecto/Flora a usar ("all" para mundial, o valores como "europe", "north-america", etc.)
 * @param client OkHttpClient (opcional, por defecto se crea uno)
 */
class PlantNetApiClient(
    private val apiKey: String,
    private val project: String = "all",
    private val client: OkHttpClient = OkHttpClient()
) {

    companion object {
        private const val BASE_URL = "https://my-api.plantnet.org/v2/identify"
    }

    /**
     * Envía una o varias imágenes (todas de la misma planta) y devuelve los resultados de identificación.
     *
     * @param images Mapa de imagen -> órgano asociado (flower, leaf, fruit, etc.).
     *               Ejemplo: mapOf(imageFile1 to "flower", imageFile2 to "leaf")
     * @return [PlantNetResponse] con los resultados parseados
     * @throws IOException si falla la red o la API devuelve error
     */
    @Throws(IOException::class)
    fun identify(images: Map<File, String>): PlantNetResponse {
        // Construir el cuerpo multipart
        val multipartBuilder = MultipartBody.Builder()
            .setType(MultipartBody.FORM)

        // Añadir cada imagen y su órgano (se puede repetir el parámetro "organs")
        for ((imageFile, organ) in images) {
            val imageBody = imageFile.asRequestBody("image/jpeg".toMediaType())
            multipartBuilder.addFormDataPart("images", imageFile.name, imageBody)
            multipartBuilder.addFormDataPart("organs", organ)
        }

        val requestBody = multipartBuilder.build()

        // Construir la URL con api-key como query param
        val url = "$BASE_URL/$project?api-key=$apiKey"

        val request = Request.Builder()
            .url(url)
            .post(requestBody)
            .build()

        // Ejecutar la llamada (síncrona, pero puedes adaptar a corutinas si prefieres)
        client.newCall(request).execute().use { response ->
            if (!response.isSuccessful) {
                throw IOException("Error HTTP ${response.code}: ${response.body?.string()}")
            }
            val jsonString = response.body?.string() ?: throw IOException("Respuesta vacía")
            return parseResponse(jsonString)
        }
    }

    private fun parseResponse(json: String): PlantNetResponse {
        // Aquí usamos Gson para parsear. Puedes usar cualquier otro (Moshi, kotlinx.serialization...)
        val gson = com.google.gson.Gson()
        return gson.fromJson(json, PlantNetResponse::class.java)
    }
}

// ---------- Data classes que reflejan la respuesta de la API ----------

data class PlantNetResponse(
    val query: Query,
    val language: String,
    val preferedReferential: String,
    val results: List<Result>,
    val remainingIdentificationRequests: Int
)

data class Query(
    val project: String,
    val images: List<String>,
    val organs: List<String>
)

data class Result(
    val score: Double,
    val species: Species
)

data class Species(
    val scientificNameWithoutAuthor: String,
    val scientificNameAuthorship: String?,
    val genus: TaxonomicNode?,
    val family: TaxonomicNode?,
    val commonNames: List<String>?
)

data class TaxonomicNode(
    val scientificNameWithoutAuthor: String,
    val scientificNameAuthorship: String?
)



THANKS ALEPH HACKATHON!, YOU REVEAL THIS SIDE OF ME IN THIS ACTIVITY, IF A LOSE THIS CONTEST, MY PROJECT WILL HAPPEN ANYWAY!
